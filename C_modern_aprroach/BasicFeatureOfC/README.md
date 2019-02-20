C现代方法的第1～10章，主要写C语言的基本特性

http://knking.com/books/c2/answers/index.html

### C语言的基本特性

将C程序转成可执行文件
+ 预处理：将程序送交预处理器proprocessor，预处理执行以 # 开头的命令，对程序进行修改
+ 编译：修改后对程序进入编译器compiler，编译器将程序翻译成机器指令（即目标代码object code）
+ 链接：链接器linker把编译器产生对目标代码和其他附加的代码整合，最终产生完全可执行程序。附加代码包括很多库函数

Unix环境下通过 ``cc punc.c``命令编译和链接c程序，系统自动链接。形成的可执行程序默认为 a.out文件。
``cc``命令可以通过 -o 选项重新命名可执行文件
```
%: cc -o pun pun.c
```

C程序依赖3个关键语言特性：指令（编译操作前修改程序的编辑命令，就是预处理器操作的 #开头指令），函数（被命名的可执行代码块），语句（程序运行执行的命令）

#### 变量

程序在产生输出前需要执行一系列计算，**变量variable** 就是在程序执行过程中临时存储数据的存储单元

有几个重要特点：

1. 类型type

类型是用来说明变量所存储的数据的种类，会影响变量的存储的方式以及允许对变量采取对操作。

2. 声明

变量在使用之前必须进行声明
```
int height; 声明语句，类型+名字

```

3. 赋值assignment

变量通过赋值 获得值
```
height = 8;

height = height * 4; 赋值运算符右侧可以是含有变量、常量、运算符的表达式expression
``

4. 初始化 initializer

通过赋值给变量一个初始值
```
int height=8; 8就是一个初始化式
```

#### 常量 constant

常量是程序执行过程总固定不变的值，可以采用宏定义的特性给常量命名

```
#define CV 9

#define就是一个预处理指令，编译期会把所有的CV都用9来替换
```


### 格式化输入与输出

#### printf

printf用来显示格式串(format string),也就是被标明格式的字符串

```
printf(格式串，表达式1，表达式2。。。)

```

格式串format string 包含普通字符串和转化说明 (conversion specification),转换说明就是用来说明被填充的位置应该怎么展示。转换说明以字符串 % 开头，%后面的信息指定了数值从内部的二进制形式转换成字符形式的方法

例如 %d表示将int数值转成十进制数字字符，%f 对float型数值进行转换

转义序列（escape sequence）：使字符串包含不能打印的特殊字符
例如 ``\n`` 换行； ``\t`` 制表符 ；

#### scanf

如同printf函数用特定格式显示输出一样，scanf函数根据特定格式读取输入。也是包括普通字符和转换说明两个部分

```
int i,j;
float x,y;
scanf("%d%d%f%f",&i,&j,&x,&y);
```

如果需要跳过前面零个或多个空白字符，需要在格式串前加入一个空格 ``scanf(" %c",&c)``

scanf采用模式匹配的方式，会从输入的数据中定位到合适的项，并跳过必要的空格。会忽略空白字符 white-space（空格符、横向纵向制表符号、换页符、换行符）


### 表达式expression

C语言更多的强调表达式expression而不是语句，表达式是显示如何计算值的公式。最简单的表达式是变量和常量。
变量表示程序运行时计算的值；常量表示不变的值。

运算符是构建表达式的工具

+ 算术运算符
    一元运算符：+ - 正负号
    二元运算符：+加、-减、*乘、/除、%取模

    ``%``要求整数操作符

多个运算符会有优先级，而通过小括号可以达到最大优先级

当一个表达式有相同优先级符号时，还要根据结合性（associativity），如果运算符是从左到右，那么是左结合，例如+、-、*、/%
如果是从右到左，是右结合的，例如 正负号+、-


（下面的都输入逻辑表达式，逻辑表达式的结果都是0或1）

+ 关系运算符

C语言的关系运算符(relational operator) 和数学上一样，但产生的结果是0（假）和1（真）。例如 10 < 11 值为1

<、> 、<= 、>=


+ 逻辑运算符

！、&&、||
非、与、或

+ 判等运算符

equality operator用来判断是否相等

``==`` 等于、``!=``不等于


赋值运算符 =

左值：表示存储在计算机内存中的对象，而不是常量或计算结果。变量是左值

自增/自减运算符

```++ i、 --i

副作用是对象加一、减一

但表达式的结果根据前缀或后缀决定，前缀是 i已经作用了，后缀是i还没作用
```


逗号运算符

```
表达式1，表达式2；
先执行表达式1并且扔掉计算的值，再执行表达式2，把这个值作为整个表达式的值。
```

C语言语句

+ 选择语句 selection statement

if语句

switch语句

```
switch(整型表达式，即必须表达式的值为整型){
    case 常量表达式 :
        expression; //执行语句
        break;     //跳出语句
    case 常量表达式:
        expression;
        break;
     default:
        expression;
        break;

}
```

+ 循环语句 iteration statement

while语句

```
while(控制表达式){

    循环体；
}
```

do语句

```
do{

}while(控制表达式);
//先执行do，再判断条件

```

for语句

```

for(初始化语句；判断表达式；控制语句)

```

+ 跳转语句 jump statement

break语句
continue语句
goto语句


### 基本类型

数值类型：整型、浮点型
整型的值全是数，浮点型的值含有小数部分

#### 整型

整型分为：有符号singed、无符号unsinged

默认是有符号整型，无符号通常用在和机器相关的应用上。

C语言的整型有不同的大小，长整型long 、整型、短整型short，为了适应不同的使用场景

三种大小 * 是否有符号 = 总共可以组合六种整型

long通常8字节,int 2（16位机）或4（32位机），char 1 字节，short 2字节

整型常量：

以文本形式显示的数，允许用十进制、八进制（以0开头）、十六进制（0x开头）书写

#### 浮点型

单精度：float （32位）
双精度：double （64位）
扩展双精度：long double


#### 字符型

char 值根据计算机不同而不同，因为不同计算机有不同字符集
**C语言会按照小整数来处理字符，也就是把char当初整型处理**

sizeof运算符

sizeof (类型名)

返回一个无符号整型，用来表示存储属于类型名的值所需要的字节数，也就是存储指定类型的值需要占多少个字节的空间，通常返回值我们用 **unsinged long**来获取

#### 类型转换

当两个数值进行运算时，会默认往大的类型进行整数提升，或者往浮点型转换

也可以通过小括号来进行强制类型转换


### 函数

函数就是一连串组合在一起并且命名的语句。每个函数本质都是一个程序。

函数定义

```
返回类型 函数名（形式参数）{

    函数体；

}

函数返回值类型：
+ 不能返回数组
+ 无返回值为void
```

可以对函数进行声明declare，使得编译器对函数进行概要浏览，完整定义放后面

```
返回类型 函数名（形式参数）；
```

C语言中参数是通过值传递的：调用函数时，将实际参数的值赋值给形式参数

### 程序结构

局部变量 localvariable

+ 自动存储期限 ：也就是说函数退出后变量被收回分配
+ 程序快作用域： 块作用域

全局变量 external variable

+ 静态存储期限：和声明为static的局部变量一样，也就是存储在外部变量的值会永久保留下来
+ 文件作用域：


作用域：变量存在的范围，新的声明会隐藏旧的声明


### 预处理器

之前说的，程序运行第一步就是预处理器进行预处理。
看一下预处理器是怎么工作的：
