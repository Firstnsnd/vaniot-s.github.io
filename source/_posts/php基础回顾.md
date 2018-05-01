---
title: php基础回顾
date: 2017-12-01 22:02:16
tags: php
categories: web
---
1.输出语句
php中的各种输出方式的总结:

函数	|说明
--|--
echo()|	echo 是指令而不是函数，可以省略(),他没有返回值，可以输出一个或多个字符串.其语法规则为:echo(string arg1[,string …])
print()|	print 其本质也是一个语言结构非函数，可省略()，有返回，只接受一个参数，输出简单的变量,无法输出数组及对象。
print_r()|	print_r() 是函数，print_r(mixed arg)打印int,boolean,float,string,及复合类型如数组、对象，成功时返回true。当设置第二个参数为true时,使print_r()返回其要输出的值而不输出,即为：print_r(mixed arg,boolean true)
die()|	die()的功能为：执行出后，退出程序。
printf()|	printf(“arg1”,arg2)执行格式化输出，arg1规定了按什么格式输出，arg2为输出的变量，格式化输出的命令及说明附后
sprintf()|	sprintf(“arg1”,arg2)执行格式化输出，arg1规定了按什么格式输出，arg2为输出的变量将输出的内容格式化，赋值给变量并不直接输出
var_export（）|	输出变量的结构信息，输出的是php的合法代码，将函数的第二个参数设置为 TRUE，从而返回变量的表示。
var_dump()|	用于显示关于一个或多个表达式的结构信息，包括表达式的类型与值。数组将递归展开值，通过缩进显示其结构。它是直接将结果输出到浏览器的，如果需要将结果保存到一个string变量中，可使用输出控制函数来捕获当前函数的输出。
<!--more-->
printf()格式化输出的命令及说明：

命令|说明
--|--
%%	|打印出百分比符号，不转换。
%b	|整数转成二进位。
%c	|整数转成对应的 ASCII 字符。
%d	|整数转成十进位。
%f	|倍精确度数字转成浮点数。
%o	|整数转成八进位。
%s	|整数转成字符串。
%x	|整数转成小写十六进位。
%X  |整数转成大写十六进位。
```
printf (“$%01.2f” , 43.2); //$43.20
$ 表示填充的字符
0 表示位数不够在不影响原值的情况下补
1 表示输出的总宽度
2 表示小数位数 ，有四舍五入
%f 是表示显示为一个浮点数
$a = array (1, 2, array ("a", "b", "c"));
var_export ($a);
/* 结果将输出：
array (
  0 => 1,
  1 => 2,
  2 => 
  array (
    0 => 'a',
    1 => 'b',
    2 => 'c',
  ),
)
*/
$b = 3.1;
$v = var_export($b, TRUE);
echo $v;
/* 结果将输出：
3.1
*/
```
2.php中的隐式转换
PHP的变量都存放在一个名为ZVAL的容器中,隐式类型转换也被称为自动类型转换，是指不需要程序员书写代码，由编程语言自动完成的类型转换。 在PHP中，我们经常遇到的隐式转换有：

1．直接的变量赋值操作
在php中,b变量的数据类型由赋予的值决定，即左值(在内存中找到地址且有名字)的数据类型由右值的数据类型决定，
```
$string = "To love someone sincerely means to love all the people,  to love the world and life,  too.";
$integer = 10;
$string = $integer;//$string的类型变为整型,$integer的引用计数加1,原右值被垃圾回收
```
2．运算式结果对变量的赋值操作
在运算的过程中发生了隐式的类型转换。
种类型转换又分为两种情况：

表达式的操作数为同一数据类型 这种情况的作用以上面的直接变量的类型转换是同一种情况，只是此时右值变成了表达式的运算结果。
表达式的操作数不为同的数据类型 这种情况的类型转换发生在表达式的运算符的计算过程中，在源码中也就是发生在运行符的实现过程中。
```
$a = 10;
$b = 'a string ';
echo $a . $b;
```
符串连接操作就存在自动数据类型转化，\$a变量是数值类型，\$b变量是字符串类型， 这里\$a变量就是隐式(自动)的转换为字符串类型了。通常自动数据类型转换发生在特定的操作上下文中，具体的自动类型转换方式和特定的操作有关。
脚本执行的时候字符串的连接操作是通过Zend/zend_operators.c文件中的如下函数进行:
```
ZEND_API int concat_function(zval *result, zval *op1, zval *op2 TSRMLS_DC) /* {{{ */
{           
        zval op1_copy, op2_copy;
        int use_copy1 = 0, use_copy2 = 0;
 
        if (Z_TYPE_P(op1) != IS_STRING) { 
                zend_make_printable_zval(op1, &op1_copy, &use_copy1);
        }           
        if (Z_TYPE_P(op2) != IS_STRING) { 
                zend_make_printable_zval(op2, &op2_copy, &use_copy2);
        }       
        // 省略
}
```
可用看出如果字符串链接的两个操作数如果不是字符串的话， 则调用zend_make_printable_zval函数将操作数转换为”printable_zval”也就是字符串。
```
ZEND_API void zend_make_printable_zval(zval *expr, zval *expr_copy, int *use_copy)
{
    if (Z_TYPE_P(expr)==IS_STRING) {
        *use_copy = 0;
        return;
    }
    switch (Z_TYPE_P(expr)) {
        case IS_NULL:
            Z_STRLEN_P(expr_copy) = 0;
            Z_STRVAL_P(expr_copy) = STR_EMPTY_ALLOC();
            break;
        case IS_BOOL:
            if (Z_LVAL_P(expr)) {
                Z_STRLEN_P(expr_copy) = 1;
                Z_STRVAL_P(expr_copy) = estrndup("1", 1);
            } else {
                Z_STRLEN_P(expr_copy) = 0;
                Z_STRVAL_P(expr_copy) = STR_EMPTY_ALLOC();
            }
            break;
        case IS_RESOURCE:
            // ...省略
        case IS_ARRAY:
            Z_STRLEN_P(expr_copy) = sizeof("Array") - 1;
            Z_STRVAL_P(expr_copy) = estrndup("Array", Z_STRLEN_P(expr_copy));
            break;
        case IS_OBJECT:
                // ... 省略
        case IS_DOUBLE:
            *expr_copy = *expr;
            zval_copy_ctor(expr_copy);
            zend_locale_sprintf_double(expr_copy ZEND_FILE_LINE_CC);
            break;
        default:
            *expr_copy = *expr;
            zval_copy_ctor(expr_copy);
            convert_to_string(expr_copy);
            break;
    }
    Z_TYPE_P(expr_copy) = IS_STRING;
    *use_copy = 1;
}
```
类似的还有求和操作”+”。

第一个操作数类型|	第二个操作数类型|	类型转换
--|--|--
整型|	浮点型|	整型转换为浮点型
整型|	字符串|	字符串转换为数字，如果字符串转换后是浮点型，整型也会转换为浮点型
浮点型|	字符串|	字符串转换为浮点型
总结:浮点型 > 整型 > 字符串

PHP是一个弱类型语言，比较的过程中是带有隐式转换的，因为你可以用一个字符串和整数进行比较，字符串会自动转化为整数,使用“===”比较符则可以避免,如果生产环境版本足够高的话，最好使用hash_equals()。
3.isset、empty、is_null的区别
- isset()如果 变量 存在(非NULL)则返回 TRUE，否则返回 FALSE(包括未定义)。变量值设置为：null，返回也是false;unset一个变量后，变量被取消了。
- empty()变量 是非空或非零的值，则 empty() 返回 FALSE。换句话说，""、0、"0"、NULL、FALSE、array()、var \$var、未定义; 以及没有任何属性的对象都将被认为是空的，如果 var 为空，则返回 TRUE
- is_null()检测传入值【值，变量，表达式】是否是null,只有一个变量定义了，且它的值是null，它才返回TRUE . 其它都返回 FALSE
>当要 判断一个变量是否已经声明的时候 可以使用 isset 函数 
当要 判断一个变量是否已经赋予数据且不为空 可以用 empty 函数 
当要 判断 一个变量 存在且不为空 先isset 函数 再用 empty 函数
4. require与include的区别
require() :如果文件不存在，会报出一个fatal error.脚本停止执行,require的使用方法如 require("./inc.php"); 。通常放在PHP程式的最前面，PHP程式在执行前，就会先读入require所指定引入的档案，使它变成PHP 程式网页的一部份。
 include() : 如果文件不存在，会给出一个 warning，但脚本会继续执行 这里特别要注意的是： 使用include()文件不存在时，脚本继续执行，include使用方法如include("./inc.php"); 。一般是放在流程控制的处理区段中。PHP程式网页在读到 include的档案时，才将它读进来。这种方式，可以把程式执行时的流程简单化。
5.php的垃圾回收