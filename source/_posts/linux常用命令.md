---
title: linux常用命令
date: 2018-01-12 14:18:19
tags: linux
categories: basic
---
## 权限
### 用户
每个用户都有对应ID(UID),至少归属于用户组(GID:同一用户组拥有相同的权利)。
```
查看id:id
查看Uid：groups
查看当前系统的用户：users
          who //查看更多详细的信息
          w //最详细信息
```
 
>who的信息

第一列|第二列|第三列
--|--|--
登录的用户名|用户登录的终端|用户登录的时间(远程登录显示用户的ip或主机名)

>w的信息

第一列|第二列|第三列|第四列|第五列|第六列|第七列|第八列
--|--|--|--|--|--|--|--
用户名|终端|网络登录时，显示主机名或ip地址|用户登录时间|用户闲置时间|与终端相关所有运行进程消耗的CPU时间总量|当前WHAT列所对应的进程所消耗的CPU的时间总量|用户当前运行的进程

调查用户：finger
```
     finger //显示系统中的登录用户
     finger vaniot //显示用户的详细信息
```
#### 用户分类
- 根用户：root对系统拥有绝对的控制权，可进行文件的任意操作
- 系统用户：系统运行时必有的用户
- 普通用户：真实用户，在授权的的目录中操作。
用户的切换
```
用户的切换：su
切换为root: su //切换用户为root,环境仍为当前用户
            su -//切换为用户root，环境也为root
退出root用户：exit
```
以root的身份执行命令：(未切换身份)
使用：sudo

sudo时，系统先会检查/etc/suddoers，判断当前用户是否有执行sudo的权限，确定有权限后，要求使用用户自己的密码验证身份

```
以root的身份运行：visudo //进入修改配置文件,在编辑保存后会自动检查语法设置，防止配置错误
在最后一行可加入：
         user ALL=(ALL) ALL //用户user 可在任意地方(地一个ALL)执行任何人(第二个ALL)的任何命令(第三个ALL,拥有了系统全部权限)
         user ALL=(ALL) NOPASSWD:ALL //用户使用sudo命令时不需要密码
         user ALL=(ALL) NOPASSWD:/sbin/shutdown,/user/bin/reboot //用户在使用此命令时不需要密码
         %group1 All=(ALL) ALL //给用户组group1赋予权限
```
#### 对用户的操作
###### 1.增加用户删除用户
```
1.增加用户：useradd john (增加名为john的用户)
            useradd -u 555 user (为用户user设置uid为555) 
            useradd -g user user2 (为用户user2指定用户组为user1)
   # 系统会在 /etc/passwd和/etc/shadow中分别记录用户名、密码，/etc/shadow默认只有root用户才有读的权限
2.修改密码：password(修改当前用户的密码)  
            password user(root修改user的密码) 
3.修改用户:usermod
            cat /etc/passwd | grep user //查看用户user的家目录,grep过滤名为user
            usermod -d /home/user_new -m user //-m检查指定用户的家目录是否存在，无就创建 /home/user_new，并使用此目录为user的新家目录
            cat /etc/passwd | grep user //查看用户user的家目录,grep过滤名为user
      冻结帐号：
            usermod -L user//冻结帐号user
            usermod -U user //解锁帐号user
4.删除用户
            userdel user //删除用户只删除/etc/passwd和/etc/shadow中的用户相关文件
            userdel user  -r //删除用户的所有个相关文件
5.增加用户组
            groupadd group1 //增加名为group1 的用户组
            cat /etc/group //查看用户组的信息
6.删除有用户组
            groupdel group1
```
##### 3.设置时间计划
   1.特定的时间执行一次：at
   
   -  设置两分钟后关机
   ![设置两分钟后关机](https://raw.githubusercontent.com/Vaniot-s/picture/master/2018-01-13%2023-08-31.png)
   ><EOT>为键盘ctrl+d输入
 
   - 设置具体时间自动关机
       ![设置具体时间自动关机](https://raw.githubusercontent.com/Vaniot-s/picture/master/2018-01-13%2023-39-07.png)
   
2.周期性执行任务：cron
   - 
### 文件操作
1.linux系统的文件结构FHS(文件层次标准)

目录|目录系统
--|--
/bin|常见的用户指令
/boot|内核启动文件
/dev|设备文件
/etc|系统和服务的配置文件
/home|系统默认的普通用户的家目录
/lib|系统函数库目录
/lost+found|ext3文件系统需要的目录，用于磁盘检查
/mnt|系统加载文件系统时常用的挂载点
/opt|第三方软件安装目录
/proc|虚拟文件系统
/root|root用户的家目录
/sbin|存放系统管理命令
/tmp|临时文件的存放目录
/usr|存放与用户直接相关的文件和目录
/media|系统用来挂载光驱等临时文件的挂载点







