---
title: redis源码解析--双向链表
date: 2019-01-06 16:57:57
tags: redis
categories: basic
---
> redis https://github.com/antirez/redis/blob/5.0/src/adlist.h
https://github.com/antirez/redis/blob/5.0/src/adlist.c
## 结构体的定义
结构体的实现,双向链表的相关定义于`adlist.h`中
节点:
```C++

typedef struct listNode {
    struct listNode *prev; //前一个节点
    struct listNode *next; //后一个节点
    void *value; //节点的值
} listNode;
```
<!--more-->
迭代器:
```C++
typedef struct listIter {
    listNode *next; //指向节点
    int direction; //迭代器的方向 迭代方向由常量AL_START_HEAD及AL_START_TAIL 指示
} listIter;
```
链表结构:
```C++
typedef struct list {
    listNode *head; //链表的头结点
    listNode *tail;//链表的尾节点

    //函数指针如类中的成员函数
    void *(*dup)(void *ptr); //复制
    void (*free)(void *ptr); //清空
    int (*match)(void *ptr, void *key); //匹配链表节点的值
    unsigned long len; //链表的长度
} list;
```
list链表结构:
![链表结构图]()
## 链表的操作
### 创建链表
创建空链表
```C
list *listCreate(void)
{
    struct list *list;

    if ((list = zmalloc(sizeof(*list))) == NULL)
        return NULL;
    //初始化属性
    list->head = list->tail = NULL;
    list->len = 0;
    list->dup = NULL;  //
    list->free = NULL; //
    list->match = NULL; //
    return list; //返回链表
}
```
### 链表的删除操作
清空链表中的元素,不删除链表本身:
```C
void listEmpty(list *list) //O(n)
{
    unsigned long len;
    listNode *current, *next; //指向当前,接下来的节点

    current = list->head; //指向头结点
    len = list->len; //链表的长度
    while(len--) { //
        next = current->next; //
        if (list->free) list->free(current->value);  //是否有自带的free方法,先使用释放当前值
        zfree(current); //释放节点
        current = next; //指向下一个节点
    }
    list->head = list->tail = NULL; //循环清空
    list->len = 0;
}

```
释放链表:
```C
void listRelease(list *list) //O(n)
{
    listEmpty(list); //释放链表中的元素
    zfree(list);//释放链表
}
```
### 在链表中添加元素
对于链表的指针添加操作.三种方法,若链表为空或添加到链表头(尾),采用`ListAddNodeHead()`或`ListAddNodeTail()`其时间复杂度为$O(1)$,不为空`listInsertNode`时间复杂度仍为$O(1)$.
头部添加
```C
list *listAddNodeHead(list *list, void *value) //O(1)头插法
{
    listNode *node; //node

    if ((node = zmalloc(sizeof(*node))) == NULL) //申请空间
        return NULL;
    node->value = value; //将链表值复制
    if (list->len == 0) {
        //第一个节点
        list->head = list->tail = node;
        node->prev = node->next = NULL;
    } else {
        node->prev = NULL;
        node->next = list->head;
        list->head->prev = node;
        list->head = node;
    }
    list->len++;
    return list;
}
```
尾部添加
```C
list *listAddNodeTail(list *list, void *value) //O(1)尾插法
{
    listNode *node;

    if ((node = zmalloc(sizeof(*node))) == NULL)
        return NULL;
    node->value = value;
    if (list->len == 0) {
        list->head = list->tail = node;
        node->prev = node->next = NULL;
    } else {
        node->prev = list->tail;
        node->next = NULL;
        list->tail->next = node;
        list->tail = node;
    }
    list->len++;
    return list;
}
```
在指定的位置添加
```C
list *listInsertNode(list *list, listNode *old_node, void *value, int after) { //O(1)将值插入指定节点后,listNode一定是存在的.
    listNode *node; //申请节点

    if ((node = zmalloc(sizeof(*node))) == NULL)
        return NULL;
    node->value = value;//节点赋值
    if (after) {
        //old_node之后插入
        node->prev = old_node; //
        node->next = old_node->next;
        //如果指定的节点是尾节点,处理尾指针
        if (list->tail == old_node) {
            list->tail = node; //
        }
    } else {
        //插入到old_node之前
        node->next = old_node;
        node->prev = old_node->prev;
        //如果指定的节点是头节点,处理头指针
        if (list->head == old_node) {
            list->head = node;
        }
    }
    //如果插入的节点不是头指针
    if (node->prev != NULL) {
        node->prev->next = node;
    }
     //如果插入的节点不是尾指针
    if (node->next != NULL) {
        node->next->prev = node;
    }
    list->len++;
    return list;
}
```
### 修改链表
将链表的尾节点移动至头结点,可用于将链表反转
```C
void listRotate(list *list) { //O(1)
    listNode *tail = list->tail; //

    if (listLength(list) <= 1) return;

    /* Detach current tail */
    //将尾节点的前驱更新为尾节点
    list->tail = tail->prev;
    list->tail->next = NULL;
    /* Move it as head */
    //将尾节点插入头节点前
    list->head->prev = tail;
    tail->prev = NULL;
    tail->next = list->head;
    list->head = tail;
}
```
### 删除节点
```C
void listDelNode(list *list, listNode *node) //O(1)删除节点
{
    //处理前驱节点指针
    if (node->prev) //不是头结点
        node->prev->next = node->next;
    else
        list->head = node->next;
         //处理后继节点指针
    if (node->next) //不是尾节点
        node->next->prev = node->prev;
    else
        list->tail = node->prev;
    if (list->free) list->free(node->value); //释放节点值
    zfree(node); //删除节点
    list->len--;
}
```
### 查找
搜索链表节点返回匹配给定键值的第一个节点.
```C
listNode *listSearchKey(list *list, void *key) //O(n)
{
    listIter iter;
    listNode *node;

    listRewind(list, &iter); //迭代器,指向头结点开始搜索
    while((node = listNext(&iter)) != NULL) { //迭代器
        if (list->match) { //是否存在匹配的函数
            if (list->match(node->value, key)) {
                return node;
            }
        } else {
            if (key == node->value) {
                return node;
            }
        }
    }
    return NULL; //不存在返回为空
}
```
返回指定索引的值,正数从链表头部为0开始,负数从链表的尾端开始-1即尾部
```C
listNode *listIndex(list *list, long index) { //O(n)
    listNode *n;

    if (index < 0) { //从尾节点开始
        index = (-index)-1;
        n = list->tail;
        while(index-- && n) n = n->prev;
    } else {
        n = list->head;
        while(index-- && n) n = n->next;
    }
    return n;
}
```
## 合并链表
```C
void listJoin(list *l, list *o) { //将
    if (o->head) //将O添加到l之后
        o->head->prev = l->tail;

    if (l->tail) //l不为空
        l->tail->next = o->head;
    else
        l->head = o->head;

    if (o->tail) l->tail = o->tail; //O不为空
    l->len += o->len;

    /* Setup other as an empty list. O为空节点*/
    o->head = o->tail = NULL;
    o->len = 0;
}
```
## 迭代器的相关操作
### 创建迭代器
创建链表的迭代器
```C
listIter *listGetIterator(list *list, int direction)
{
    listIter *iter; //

    if ((iter = zmalloc(sizeof(*iter))) == NULL)  return NULL; //创建
    if (direction == AL_START_HEAD) //根据迭代器的方向，将迭代器的指针指向表头
        iter->next = list->head;
    else //或者表尾
        iter->next = list->tail;
    iter->direction = direction; //记录方向
    return iter;
}
```
### 修改迭代器
将迭代器指向链表头,并正向
```C
void listRewind(list *list, listIter *li) { //O(1)
    li->next = list->head;
    li->direction = AL_START_HEAD;
}
```
将迭代器指向链表为,并逆向
```C
void listRewindTail(list *list, listIter *li) { // O(1)
    li->next = list->tail;
    li->direction = AL_START_TAIL;
}
```
### 迭代器执行
返回迭代器指向的元素,并移动迭代器
```C
listNode *listNext(listIter *iter) //O(1)返回迭代器指向的元素,并移动迭代器
{
    listNode *current = iter->next; //指向iter指向的节点

    if (current != NULL) {
        if (iter->direction == AL_START_HEAD)
            iter->next = current->next;
        else
            iter->next = current->prev;
    }
    return current; //返回当前迭代器指向的节点
}
```
## 使用迭代器操作链表
### 使用迭代器拷贝链表
 复制整个列表。在内存不足返回NULL。
```C
list *listDup(list *orig)  //拷贝链表
{
    list *copy; //链表指针
    listIter iter; //迭代器
    listNode *node; //节点

    if ((copy = listCreate()) == NULL) //创建链表
        return NULL;
        //复制操作
    copy->dup = orig->dup; 
    copy->free = orig->free;
    copy->match = orig->match;
    listRewind(orig, &iter); //将迭代器指向链表头
    while((node = listNext(&iter)) != NULL) { //迭代器指为空
        void *value;

        if (copy->dup) { //是否有复制的函数
            value = copy->dup(node->value);//复制节点的值,返回复制节点的值
            if (value == NULL) {//值为空,复制失败
                listRelease(copy); //清除链表
                return NULL;
            }
        } else
            value = node->value; //没有复制的函数,赋值操作
        if (listAddNodeTail(copy, value) == NULL) { //将值添加到尾端
            listRelease(copy);
            return NULL;
        }
    }
    return copy;
}
```
