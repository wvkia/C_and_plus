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

具有静态存储期限的变量必须是常量

static int i= 2;

```


#### 声明
声明是为编译器提供有关标识符含义的信息

```
声明说明符   声明符；

声明说明符(declaration specifier)描述声明的数据项性质
声明符(declarator)给出数据项的名字

声明说明符：

+ 存储类型 auto、static、extern、register。
+ 类型限定符 const volatile
+ 类型说明符 void 、char、short、int、long、float、double、signed、unsigned，包括其中的组合

存储类型  类型限定符 类型说明符            声明符
extern   const    unsigned long int     a[10];

```
然后对三种说明符进行解释：

存储类型：说明变量的存储期限、作用域以及链接
存储期限表示：系统为变量预留和释放内存的时间。自动存储期限的变量在所处块被执行是获取内存，块终止时释放内存；而静态存储期限在程序运行期间有相同的存储单元，也就是可以允许变量无限期保留值
作用域：引用变量的范围
链接：确定变量共享的范围。外部链接的变量可以被多个文件共享，内部链接的变量只能单独属于一个文件，无链接的变量只能属于一个函数

auto：自动存储期限，块内部变量
static：可用于全部变量，但块外部声明和块内部声明会有不同的效果。当块外部时，static说明变量是内部链接，只能本文件使用，不能被共享，相当于private；
    当在块内部的，static变量将存储期限有auto变为静态，而且会一直保留值，也就是下一次进入函数时，值于上一次一样，不会被重新初始化
    而且当块的static变量时，只会在程序第一次进入初始化，而auto会每次都初始化；函数不能返回auto变量的指针，因为每次都会被初始化，但可以返回static变量指针

extern：多个文件共享一个变量
``extern int i;``这样，编译器不会给i分配内存，因为这句只是提示i在别处定义了，变量可以有多次声明，但只能有一处定义

函数的存储类型只有static和extern ，extern说明函数具有外部链接，也就是允许其他文件调用，static说明只能内部文件调用


类型限定符：const、volatile。
const说明变量是只读的，可以访问不能改变

对于复杂声明：
例如``int *(*x[10])(void);``
规则：
+ 从内往外读声明符
+ 先是[]、()，然后是*

那么上面x是个数组 -> 读到*，指针指向 -> 右边的括号，函数，参数void的函数 -> 左边int*，返回指向int型的指针

x是一个10个元素的数组，数组元素都是指针，且是函数指针，这个函数返回int*，参数void



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
switch(整型表达式，即必须表达式的值为整型，即整型、字符、枚举){
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

之前说的，程序运行第一步就是预处理器进行预处理，在编译前编辑C程序
看一下预处理器是怎么工作的：

预处理器的行为由**指令**控制，指令是由#字符开头的一些命令。
#define 指令定义一个宏--用来代表其他东西的一个名字，通常是某一类型的常量。预处理器会将宏的名字和它的定义存储在一起来响应#define指令。当这个宏在后面程序使用到，预处理器就将
宏替换为它定义的值

#include指令告诉预处理器打开一个特定的文件，将它的内容作为正在编译的文件的一部分包含进来，例如``#include <stdio.h>``指示预处理器打开一个名字为stdio.h的文件，并将它的内容加到
当前的程序中

预处理器的输入是一个C程序，程序中可能包含指令。预处理器会执行这些指令，并处理删除这些指令，而输出是另一个程序：被编辑后的程序，不再包含指令，然后输出被直接给到编译器，并翻译为目标代码（机器指令）
```
C程序 -> 预处理器 -> 修改后的C程序 -> 编译器 -> 目标代码
```

##### 预处理器指令

类型：
+ 宏定义 #define指令定义一个宏，#undef指令删除一个宏定义
+ 文件包含 #include指令指定一个文件内容包含到程序中
+ 条件编译 #if、#ifdef、#ifndef、#elif、#else、#endif指令根据测试条件来将一段文本块包含到程序或排除到程序之外

规则：
+ 指令都以#开头
+ 指令在第一个换行处结束，除非明确指明继续
```
通过``\``来指明指令继续
#define N (S * \
            T * C \
            d)
```

####宏定义

宏的本质就是文本替换

简单宏
```
#define 标识符 替换列表
```
```
注意，不要宏用等于号

#define N=10 是错误的

也不要用 ; 来结束
#define N 10;也是错误

编译器会报错，但是是因为N会被替换为 10; 而报错，并不会直接找到宏定义错误
```

带参数的宏

```
#define 标识符(x,y,...z) 替换列表
```

当预处理器遇到参数宏，会将定义存储，后续会将标识符格式进行宏调用
```
#define MA(x,y) ((x) - (y))

如果是 i = MA(j+k,m-n)
会被替换为
i = ((j+k) - (m-n))


```

带参数的宏往往会作为简单函数使用

宏定义可以包含两个运算符: # 和##，预处理器会处理它们

#运算符将宏参数转换为字符串字面量，例如
```
#define PRINTX(x) printf(#x " = %d\n",x)

如果是PRINT(i/j)，会被替换为

printf("i/j" " = %d\n",i/j)

注意原先宏x的位置会被替换为i/j，但前面的#会将i/j给字符串化
```

## 运算符将两个记号粘在一起，记号粘合
```
#define MID(n) n##n
如果是 MID(i+j),会是

i+ji+j


```

宏的属性：
+ 宏的替换列表可以包含另一个宏的调用

```
#define PI 3.14
#define TWO_PI (2 * PI)

后面程序如果是TWO_PI会被替换成 (2*3.14)
```

关键点⚠️：宏定义的每一个参数都必须加上小括号，限制替换范围


#### 条件编译

条件编译是根据预处理器执行的测试结果来包含或排除代码片段

```
#if 测试
语句
#endif

例如
#define DEBUG 1
#if DEBUG
printf("value i ",i);
#endif

#defined 指令检测是否已经被定义了，返回0或1

通常是和 #if结合，也用#ifdef代替，也就是

#ifdef DEBUG 如果定义了DEBUG这个宏
代码
#endif


#if 表达式

#else

#endif

```

### 字符串

字符串本质就是字符数组，后加'\0'表示结尾

字符串字面量是作为数组存储的，编辑器会将其看作 char * 类型的指针。

所以``printf("abc")``，printf函数传递的是"abc"的地址，即第一个字符的存储单元地址

注意 "a"和'a'的区别，"a"是一个字符串，也就是字符数组，而'a'仅仅是一个字符，内部存储是一个整数

可以使用puts函数写字符串，gets函数读字符串

```
<string.h>函数库提供操作字符串的函数，每个函数都需要一个字符串作为实际参数

char * strcpy(char *s1, char *s2) 将s2的字符串复制给s1，也就是用s2的字符数组元素占据s1的位置，直到遇到s2的一个空字符为止

char * strcat(char *s1, char *s2); 将s2的内容追加到s1的末尾，并且删除s1的'\0',返回最新的s1地址

int strcmp(char *s1, char *s2); 比较s1和s2的大小，利用字典顺序比较

size_t strlen(char * s1); 计算s1的长度，也就是字符个数，不包括最后的'\0'


```

### 代码组织

头文件


用于放公共的函数声明和外部变量，使用#ifndef来防止重复

```
#include "文件名"      //用于自己编写的文件，会到当前目录下找
#include <文件名>      //用于C库头文件，会到系统头文件目录找


头文件格式

#ifndef ABC_H //名字用头文件名称命名
#define ABC_H //防止重复导入include

#define N 10
int abc = 10;

void f(char *);
//代码块

#endif

然后在使用头文件的时候

#include "abc.h"
int main(){
    extern int abc; //声明外部变量

}

```

这样一个程序被拆成多个文件，方便管理和使用，在编辑和链接程序的时候和一个程序一样
+ 先每个文件单独编译
+ 在所有的目标文件链接在一块

我们通常使用makefile来减少工作量

makefile描述文件之间的依赖性，然后自动编译和链接

下面是一个典型的makefile

```
fmt: fmt.o line.o           //每组的第一行给出目标文件，第二行给出命令 fmt程序依赖 fmt.o和line.o ,而两个目标文件的编译通过下面的命令实现
    cc -o fmt fmt.o line.o
fmt.o: fmt.c line.h
    cc -c fmt.c

line.o : line.c line.h
    cc -c line.c


```









### 结构、联合和枚举

结构（structure）可以具有不同类型的值（成员member）的集合
联合（union）和结构类似，但联合的成员共享同一存储空间，结果就是每次存储一个成员，空间大小由最大的那个成员决定
枚举（enum）是一种整数类型

结构
```
struct{
    int a;
    char b[N];
}part1,part2;

相当于匿名结构，只用于part1和part2两个变量

结构成员内存中是按声明顺序存储的

结构的初始化可以直接通过{}来初始化值 part1={23,"asdf"};

访问结构的成员使用 ** . ** 运算符, part1.name

结构可以使用 = 来复制，产生相同结构， part2=part1;


通过声明结构标记

struct part{
    int a;
    char b[N];
};
struct part part1,part2;
```

和结构不同，联合成员存在同一内存地址

```
union U {
    int a;
    float b;
}u;

这时，u占用4个字节，而a是2个字节，b是4个字节，成员ab之间用一个地址
```
但注意因为计算机可能存在字节对齐的要求，所以可能会出现结构的字节大小与成员相加不一致
例如
```
struct{
    char a;
    int b;
}s;

这时候如果计算机要求4字节对齐，也就是必须用4的倍数来存储数据，那么char 是1个字节，int是4个字节，为了满足4字节对齐，不可能是s为5个字节，而应该是离5最近4的倍数
也就是8个字节，那么sizeof(s)就是8字节。就是a后面有3个字节空洞，然后是4字节的b

所以我们最好在排序结构成员的时候，满足字节对齐，避免空洞
```


枚举
枚举是一种程序员可以自定义的整数类型
```
enum suit{
    A,B,C
} ;

enum suit s1,s2;声明枚举值

枚举值可以赋值，例如
enum suit{
    A=1,B=3,C=2
};
默认不赋值的化，后面的C大于B大于A
```



### 标准库介绍

```
<ctype.h> 字符处理
<float.h> 浮点型特性
<limit.h>整数特性，提供整数类型以及字符类型的宏
<math.h> 数学处理,
<signal.h>信号处理 ，提供用于异常情况处理的函数，包括中断和运行时错误
<stdio.h> 输入输出
<stdlib.h> 常用实用程序，包括随机值、内存管理任务、操作系统通信、搜索排序、多字节字符以及字符串操作
<string.h> 字符串处理
<time.h> 日期和时间
```


