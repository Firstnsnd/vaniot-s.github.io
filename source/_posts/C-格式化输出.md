---
title: C++格式化输出
date: 2018-02-04 02:01:01
tags: C++
categories: basic
---

#### 在输入输出流中使用控制符
输入输出流中的控制语句

  控制符|作用
  ---|---
  dec|设置数值的基数为10
  hex|设置数值的基数为16
  oct|设置数值的基数为8
  setfill(c)|设置填充字符c，c可以是字符常量或字符变量
  setprecision(n)|设置浮点数的精度为n位,在以一般十进制小数形式输出时，n代表有效数字。在以fixed(固定小数位数)形式和 scientific(指数)形式输出时，n为小数位数
  setw(n)|设置字段宽为n
  setiosflags(ios::fixed)|设置浮点数以固定的小数位数显示
  setiosflags(ios::scient)|设置浮点数以科学记数(指数形式)显示
  setiosflags(ios:left)|输出数据左对齐
  setiosflags(ios::right)|输出数据右对齐
  setiosflags(ios::skipws)|忽略前导的空格
  setiosflags(ios::uppercase)|数据以十六进制形式输出字母以大写表示
  setiosflags(ios::lowercase)|数据以十六进制形式输出时字母以小写表示
  setiosflags(ios::showpos)|输出正数时给"+"号

> 如果使用了控制符，在程序单位的开头除了要加iostream头文件外，还要加iomanip头文件。

<!--more-->

#### 使用格式输出的头文件:iomanip

函数：

 流成员函数|相同作用的控制函数|作用
  ---|---|---
  precision(n)|setprecision(n)|设置实数的精度为n位
  width(n)|setw(n)|设置字段的宽度为n位
  fill(c)|setfill(c)|设置填充字符c
  setf()|setiosflags()|设置输出格式状态，括号中应给出格式状态设置标志，内容与控制符setiosflags括号中的内容相同
  unsetf()|resetioflags()| 终止已设置的输出格式状态，在括号中给出设置输出状态的标志符
设置格式状态的格式标志：
     流成员函数setf和控制符setiosflags括号中的参数表示格式状态，它是通过格式标志来指定的。格式标志在类ios中被定义为枚举值。因此在引用这些格式标志时要在前面加上类名ios和域运算符“::
 
  格式标志|作用
  ---|---
  ios::left|输出数据在文本域宽范围内向左对齐
  ios::right|输出数据在文本域宽范围内向右对齐
  ios::internal|数值的符号位在域宽内左对齐,数字右对齐，中间由填充字符填充
  ios::dec|设置整数的基数为10
  ios::oct|设置整数的基数为8
  ios::hex|设置整数的基数为16
  ios::showbase|强制输出整数的基数(八进制数用0打头,十六进制以0x打头)
  ios::showpiont|强制输出浮点数的小点和位数0
  ios::uppercase|在以科学计数法格式E和以十六进制输出字母时以大写表示
  ios::showpos|对正数显示"+"号
  ios::scientific|浮点数以科学计数法格式输出
  ios::fixed|浮点数以定点格式(小数形式)输出
  ios::unitbuf|每次输出后刷新所有的流
  iosL::stdio|每次输出后清除stdout，stderr
```
    #include <iostream>
    #include <iomanip>//不要忘记包含此头文件
    using namespace std;
    int main()
    {
       int a;
       cout<<"input a:";
       cin>>a;
       cout<<"dec:"<<dec<<a<<endl;  //以十进制形式输出整数
       cout<<"hex:"<<hex<<a<<endl;  //以十六进制形式输出整数a
       cout<<"oct:"<<setbase(8)<<a<<endl;  //以八进制形式输出整数a
       char *pt="China";  //pt指向字符串"China"
       cout<<setw(10)<<pt<<endl;  //指定域宽为,输出字符串
       cout<<setfill('*')<<setw(10)<<pt<<endl;  //指定域宽,输出字符串,空白处以'*'填充
       double pi=22.0/7.0;  //计算pi值
       //按指数形式输出,8位小数
       cout<<setiosflags(ios::scientific)<<setprecision(8);
       cout<<"pi="<<pi<<endl;  //输出pi值
       cout<<"pi="<<setprecision(4)<<pi<<endl;  //改为位小数 4位小数
       cout<<"pi="<<setiosflags(ios::fixed)<<pi<<endl;  //改为小数形式输出
       return 0;
    }
```
```
#include <iostream>
using namespace std;
int main( )
{
   int a=21
   cout.setf(ios::showbase);//显示基数符号(0x或)
   cout<<"dec:"<<a<<endl;         //默认以十进制形式输出a
   cout.unsetf(ios::dec);         //终止十进制的格式设置
   cout.setf(ios::hex);           //设置以十六进制输出的状态
   cout<<"hex:"<<a<<endl;         //以十六进制形式输出a
   cout.unsetf(ios::hex);         //终止十六进制的格式设置
   cout.setf(ios::oct);           //设置以八进制输出的状态
   cout<<"oct:"<<a<<endl;         //以八进制形式输出a
   cout.unseft(ios::oct);
   char *pt="China";              //pt指向字符串"China"
   cout.width(10);                //指定域宽为
   cout<<pt<<endl;                //输出字符串
   cout.width(10);                //指定域宽为
   cout.fill('*');                //指定空白处以'*'填充
   cout<<pt<<endl;                //输出字符串
   double pi=22.0/7.0;            //输出pi值
   cout.setf(ios::scientific);    //指定用科学记数法输出
   cout<<"pi=";                   //输出"pi="
   cout.width(14);                //指定域宽为
   cout<<pi<<endl;                //输出pi值
   cout.unsetf(ios::scientific); //终止科学记数法状态
   cout.setf(ios::fixed);        //指定用定点形式输出
   cout.width(12);               //指定域宽为
   cout.setf(ios::showpos);      //正数输出“+”号
   cout.setf(ios::internal);     //数符出现在左侧
   cout.precision(6);            //保留位小数
   cout<<pi<<endl;               //输出pi,注意数符“+”的位置
   return 0;
}
```

#### 使用stdio.h
printf()操作字符串的可以有：
- 字符串的左、右对齐
- 字符串打印长度的设置
- 字符串填充符的前后设定
- ‘char’与数字的相互转换
```
  // 正常的打印字符串
    printf("%s|end\n", "test123");
    // 设定打印的%s字符串长度最小值为10，长度不足10时默认在字符串的左边填充' '空格。
    printf("%10s|end\n", "test123");
    // 设定打印的%s字符串长度最小值为10，长度不足10时默认在字符串的右边填充' '空格。
    printf("%-10s|end\n", "test123");
    // 设定打印的%s字符串长度最小值为10，长度不足10时默认在字符串的左边填充'0'空格。
    printf("%010s|end\n", "test123");
    // 打印出ASCII码表中数字所代表的字符
    printf("%c%c%c%c%c %d %d %d %d %d.\n", 72, 101, 108, 108, 111, 'H', 'e', 'l', 'l', 'o');
// output
test123|end
   test123|end
test123   |end
000test123|end
Hello 72 101 108 108 111.
```
1.设定数字的输出进制表达式

    o:八进制
    d:十进制
    u:无符号数的十进制
    x:十六进制(字母部分大写)
    X:十六进制(字母部分小写)
```
  printf("%o %d %u %d %x %X\n", 123, 123, -123, -123, 123, 123);
// output
173 123 4294967173 -123 7b 7B
```
操作数字打印的长度及精度
操作数字打印的左、右对齐方式
```
// %f默认小数点后6位数
    printf("%f|end\n", 12.3);
    // %f默认小数点后2位数
    printf("%.2f|end\n", 12.3);
    // %f默认小数点后2位数，打印的字符串长度最小值为6，不足的个数用' '填充在左边
    printf("%6.2f|end\n", 12.3);
    // %f默认小数点后2位数，打印的字符串长度最小值为6，不足的个数用' '填充在左边
    printf("% 6.2f|end\n", 12.3);
    // %f默认小数点后2位数，打印的字符串长度最小值为6，不足的个数用'0'填充在左边
    printf("%06.2f|end\n", 12.3);
    // %f默认小数点后2位数，打印的字符串长度最小值为6，不足的个数用' '填充在右边
    printf("%-6.2f|end\n", 12.3);
// output
12.300000|end
12.30|end
 12.30|end
 12.30|end
012.30|end
12.30 |end
  // 默认小数点后6位，e小写
    printf("%e \n", 1.23f);
    // 默认小数点后6位，E大写
    printf("%E \n", 1.23f);
    // 设置小数点后2位，总长度最小值为10，不足默认用‘ ’空格在左边填充，E大写
    printf("%10.2E|end \n", 1.23f);
    // 设置小数点后2位，总长度最小值为10，不足默认用‘ ’空格在左边填充，E大写
    printf("% 10.2E|end \n", 1.23f);
    // 设置小数点后2位，总长度最小值为10，不足默认用‘0’在左边填充，E大写
    printf("%010.2E|end \n", 1.23f);
    // 设置小数点后2位，总长度最小值为10，不足默认用‘ ’空格在右边填充，E大写
    printf("%-10.2E|end \n", 1.23f);
// output
1.230000e+00 
1.230000E+00 
  1.23E+00|end 
  1.23E+00|end 
001.23E+00|end 
1.23E+00  |end
```
