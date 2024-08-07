# C语言基础
!!! note
    - 课程：程序设计与算法基础
    - 任课老师：翁恺
> 由于只上了一学期C语言，很多内容都是浅尝辄止，当时的笔记记的也很随意，如果以后有机会更深入学习C语言，可能会进一步更新笔记
## C语言基础总结

### 常识
- **真为1假为0** 
  - 例如3<x<5恒等于1     *先看3<x,等于0或1，小于5*
- **表达式一定会有一个结果，以及可能产生副作用**
  - 比如`int a=2++`,这个表达式的结果是使2的值自增为3，副作用是把2的值赋给a，即`a=2`
  - 比如`if(x=0)`,表达式结果是0，所以if语句永不执行；副作用是把0赋给x，即`x=0`


### C语言标识符
- 标识符必须以**字母、下划线**或美元符号$开头，**不能以数字开头**；
- 标识符只能由**字母、数字、下划线**或美元符号组成，不能使用其他符号；
- 标识符的长度不能超过 63 个字符；
- C语言**是区分大小写的**，因此变量 a 和变量 A 是两个不同的变量；
- C语言中有一些关键字不能用作标识符，如 if、else、while、for 等。


###  优先级
注意以下几点：
1. 比较大于计算大于赋值
2. 判断运算符中，否>并>或
3. 单目>双目
4. `[ ]`的优先级也是一级
5. `++`>`*`

#### 结合性
> 结合性是用来判断具有相同优先级的运算符组合式的计算顺序
##### 右结合
- 从右往左计算
- 包括赋值运算符
##### 左结合
- 从左往右计算
- 包括算数运算符，逻辑运算符

### 重要算法
- 模拟竖式除法
```c
//digit为位数，n为被除数,x为光棍数
      int n,digit=1,x=1;
      scanf("%d",&n);
//先找到比n大的最小x
      while(n>x){
            x=10*x+1;
            digit++;
      }
     while(1){ 
//逐位输出
            printf("%d",x/n);
            if(x%n==0){
                  break;
            }
            x=x%n*10+1;
            digit++;
      }
      printf("\n%d",digit);
```
- 求交错项
```c
//定义符号变量
int sign=1;
for(;;){
    sum+=sign*value;
    sign=-sign;//每轮变号
}
```
### 错题整理
1. 注释符号是`/*  */`
2. 循环体如包括有一个以上的语句，则必须用一对大括号{}括起来，组成复合语句，复合语句在语法上被认为是一条语句。
3. `do{}while();`语法错误，因为`while`括号里必须有内容；`for(;;);`语法正确，因为for循环三个表达式都可以省略，但注意` ; `不能省
4. 函数只能获得一个返回值说法错误
> 函数有0或1个返回值
5. float类型的加减法不符合二进制(binary)，0.1+0.2！=0.3
6. `sizeof`不是函数,不要在里面做运算，做了也没有结果，如`int a=4;printf("%lu,%d",sizeof(a++),a)`得到的结果为4,4,即a没有自增（运算未发生）
> is a static operator which does its calculation during compilation
7. ld报错:编译已通过，编译是让源代码产生.c文件，main不是关键字，打错main会产生ld报错
### 英文
loop 循环

variables 变量

value to x x的值

precedence 优先

Operator 运算符

ascended/descended order 升序/降序

 enumerates 列举

## 数组
### 定义数组
**< type >+name[元素数量 ]**  *eg:int weight[number]*
注意：

1. 【】中可以是变量或者参数，我们一般定义一个常数变量放里面，如*const int number=10*
2. 数组可以看成一个储存容器，每一个都有一个值且具有相同元素类型，可以是int或double等类型，可以输入输出；但【】中表示元素数量，只能是整数
3. 从0开始
4. 数组的大小在创建时确立，之后无法改变
5. 数组的大小可以为0，但不能存放任何数值
### 初始化数组
**方法一：遍历数组，用for循环初始化**

**方法二：数组的集成初始化**

1. **int a[]={1,4,6,7,4,56,7,}**   *表示一共有七个元素，依次赋值*
2. **int a[12]={1,2,}** *共有12个元素，表示第0个和第一个一次赋值为1，2；其他一律为0*
3. **int a[12]{ [2]=1,[5]=2,4}** *表示第二个赋值为1，第五个赋值为2，第五个后面也就是第六个赋值为4，其余为0*
4. 数组中元素个数可用 **sizeof(a)/sizeof(a[0])** 表示
**注意：数组大小必须是字面量，不能是变量，此时才能集成初始化**
### 使用数组
1. 数组作为函数的参数时，不能在【】给出数组的大小，不能用sizeof计算数组的数量
2. const在*前面，指针指向常量，不能修改指针指向的值
3. const在*后面，指针是常量，指针储存的位置不能修改
### 二维数组
定义`int a[10][10]`**两个数量都要声明**或者`int a[][10]={0}`**集成初始化**，第一位的数量可以不给出

## string
### 输入
```c
char a[20]
scanf("%s",a);
```

1. 不用取地址符，因为a代表a[0]的位置
2. 读到第一个空格，制表符或换行符时不再读入，**计算机自动为末尾添加\0**
3. %s会读取**除空白**外的字符，会跳过空白读第一个非空白字符
4. %d每次读取的是上一次读取丢弃的非数字字符

- " "标识字符串，‘ ’表示单个字符，字符串以\0结尾（ASCLL码为0）
### sizeof and strlen
#### sizeof
- sizeof is not a function but a unary operators
- 对于数据类型，必须加括号，如  `sizeof(int)` ;对于特定量，括号可加可不加，如`sizeof name等同于sizeof(name)，sizeof 8等同于sizeof(8)`
- 未知存储大小的数组类型、未知内容的结构或联合类型、void类型等是不正确的. 
- 当操作是具体字符串或数值，sizeof会根据类型进行相应转化
  `例如 sizeof(2)=4
    sizeof("aab")=4`
#### strlen
### 预处理
`# define NAME value`
- **末尾不用分号**
- 命名规则和变量一样，数字字母下划线，不能数字开头
- const 是创建了不能修改的变量
### printf and scanf
- `scanf("%c",a)`从输出的第一个字符开始读；`scanf(" %c",a)`从第一个非空白字符开始读
- `printf("%5d",a)`表示输出五个字符长度，若a大于5个长度，会自动扩充为a的长度；若a小于5个长度，会输出空格来达到5个长度（右对齐）；`printf("%-5d",a)`表示左对齐
## 结构struct
### 定义 
- `struct name{};`然后把struct name 看作一个新的数据类型
- 定义的两种方式
```c
struct date thisday{
    int x;
    int y;
    double a;
};//注意分号不要漏

struct date thisday{//此处thisday也可以省略
    int x;//也可以 int x,y;
    int y;
    double a;
}p1,p2;//同时创建了p1,p2两个结构变量
```

### 初始化
```c
struct date a={1,2};
a.x=3;
```
### 易错
- p是指向结构的指针，则 `(*p).year` 等同于 `p->year`

- &
    - 只能取变量的地址，而不能取一个结果的值
    - %p
- `int* p,q`等于`int *p,q`,其中q是整形而**不是指针**
- *在定义变量是指针标记，其他时候是做运算符（乘法或解引用）
### scanf的返回值
- 表示成功读入的值的个数

- 读到第一个错误类型就会停止继续读入
    ```c
    int i,j,k;
    //读入abc,123
    k=scanf("%d %d",&i,&j);
    //读到abc就出错，停止继续读入
    printf("%d %d %d",i,j,k);
    //结果：k=0
    ``` 
--------
## 指针pointer
### 与数组的关系
#### 区别与联系
- 数组作为函数参数时就是指针
  
  ```c
  //以下等价
    int sum(int *ar, int n);
    int sum(int *, int);
    int sum(int ar[], int n);
    int sum(int [], int);
  ```

- 数组名可以看作常数指针，因此不能被赋值，不能做++运算等
    - 数组只能在定义时赋值，定义后不能在赋值，如：
    ```c
    char str[]="string";//正确
    char str[10]; str="string";//错误,数组是const pointer
    char *p; p="string";//正确，普通指针不是const pointer
    ```
####  `int a[10]`的理解
 
- 创建了一个数组a,其中a的类型是**数组名**，也就是`int [10]`,而不是`int *`
    - 数组名在大多数情况下会被**自动转换为指向数组第一个元素的指针**，这是因为数组名和指针之间存在一种**隐式**的关系。因此，当我们使用数组名时，它会被解释为指向数组第一个元素的指针。
    - `a=&a[0]`
    - a存储的是a[0]的地址，**但a代表的是整个数组**。因此&a和a虽然数值相同但意义不同，可以从`(&array)+1`和`array+1`中看出------前者移动了40个Byte，后者移动了4个Byte

- [ ]可以看作一个特殊加法运算，`i[a]=a[i]=*(a+i)`
    - 指针也可以用[ ]    

#### *与[ ]的组合
- [ ]的计算优先级要高于 *
- `int *p[10]`=`int *p([10])`
    - p表示一个数组，其中每一个元素都是一个指针，即p的类型是指针**数组**
    ```c
    int a=1,b=2;
    int *p[2]={&a,&b};
    //输出a的值
    printf("a=%d",*p[0]);
    ```
- `int (*p)[10]`
    - p是一个指针，他指向一个数组，即p的类型是数组**指针**
    ```c
    int arr[2]={1,2};
    int (*p)[2]=&arr;
    //输出arr中的第一个元素
    printf("%d",(*p)[0]);
    ```

### dangling pointer
- 指针未初始化，可能指向任意位置，可能造成严重后果

### pointer and calculable
- 指针+1是指向下一个变量
- 如果指向的不是一段连续的空间，如数组，则这种计算没有意义
- 以下运算可以做
    - 加减一个常数（+，-，+=，-=）
    - 递增递减（++，--）
    - 两个指针相减,注意**不能相加**
    - 同类型的指针可以比较大小,比较的是指针中储存的地址的大小
#### `*p++`
- *的优先级和++相同，但是右结合
    - 等同于`*(p++)`
- 先取出指针p所指的那个值，然后再让指针指向下一个 

### 指针类型转化
#### `void *`
- 不知道指向什么东西的指针
- 计算时与`char *`类似，即一个字节
#### 类型转化
- 如 `int *p=&i;void *q=(void *)p`
- 并没有改变p所指变量的类型，但用不同的方式访问p所指的内存
#### 指针类型的深入理解
- 指针的地址是字节的首地址
    - 不同类型的指针读取字节的方式不同，比如char只读一个字节，而int连读四个字节
    - 这也是指针类型转化的原理，改变读字节的方式，但没有改变储存的数据

### 0地址
- 0地址不可读也不可写，即0地址不可访问
- NULL是一个预定定义的符号，表示零地址

------------------------------

## 字符串string
- C语言的字符串是以字符数组的形式存在的
- 初始化方式
```c
char *s={'a','b','c','\0'};//注意最后有0，'\0'==0!='0'
char *s="abc";//字面量直接赋值，编译器自动添加结尾的0
//below is wrong because the pointer is a dangling pointer
char *s;
scanf("%s",s);
```
### `char *p` and `char p[]`
- 前者是一个**常量指针**，初始化后不能再修改
    - 原因是这个字符串指针在代码区而不是栈区，只可读不可写
- 前者也不一定是字符串，除非他所指的字符串以\0结尾

### string operation
#### assignment
- ```c
    char *t="tittle"
    char *s;
    s=t;
  ```
- s and t are two pointers which all point to the string *tittle*

## 全局变量与局部变量
### 内存的结构
- heap （堆）：用于动态申请内存
- stack（栈）:局部变量
- static/global：全局变量和静态局部变量
- code（代码段，只读）：如指向字符串的常数指针

### 全局变量
- 定义在**函数外**的变量
- 作用域是**整个程序**，而不是单个`.c`文件
- 可以与局部变量重名，若重名，在函数中优先使用局部变量
     ```c
        int x = 5, y = 6;
        void incxy( )
        {   
            x++;
            y++;
        }
        int main(void )
        {   
          int x = 3;
          incxy( );
          printf("%d, %d\n", x, y);
        }
    ```
    - ***输出为x=3,y=7***
      - 对这个程序，因为y是全局变量，所以函数会改变它的值；
      - x全局变量与局部变量同名，优先使用局部变量，故x的值不变；

#### 全局变量的初始化
- 只能用当前已知的值来初始化
- 若没有初始化，则会自动初始化为**NULL**或**0**
- 初始化在main函数之前
### `static`
- 变量前加上`static`变成静态变量
- 静态变量实际上是全局变量，储存在global区,即其生存区和全局变量一样
- 静态是使**作用域变成局部**
  - 静态局部变量在函数内部，只被**初始化一次**。当函数离开时，静态本地变量会继续存在并保持原值，例如
    ```c
    void f(){
      static int a=2;
      a+=2;
      printf("%d",a);
    }
    ```
    - 连续调用三次f函数，输出为4，6，8
  
  - 静态本地变量作用域只在单个`.c`文件中，而不是整个程序
  - 静态函数是只能在当前编译单元使用的函数（同静态全局变量）

### 自动变量和外部变量
- 自动变量是自动创建和销毁的变量，局部（本地）变量是自动变量
- 外部变量是多个模块可访问的变量，全局变量是外部变量
#### 区别
1. 可见性：外部变量全局可见
2. 生存期和作用域
3. 初始化：外部变量自动初始化为0，自动变量需手动初始化
   
------

## 程序结构 

### 编译预处理
- `#`开头的语句是**编译预处理指令**
- 不是C语言特有而是所有语言共有

#### 宏
- `#define name expression` 用来定义一个宏
- 如果要换行，行末需加`\`
##### 宏定义的函数
- 宏可以定义成像函数一样，如`#define cube(x) ( (x)*(x)*(x) )`
    - 注意表达式中每个x都要加括号，表达式整体也要加括号
    - 可以带多个参数，也可以组合定义其他宏
##### 运算符
1. `#` operator
- 将参数替换为字符串输出,`#a`->`"a"`
  - `# define prt(a) printf(#a "=%d",a)`,则替换为`printf("a" "=%d",a)`
1. `##` operator
- 把两个字符串连接起来
  - 如：`#define ME(item,id) item##_##id`,则替换为`item_id`
##### 条件编译

```c
    #ifdef NameofMacros
    #else//else也可没有
    #endif
    //或者：
    #ifndef NameofMacros
    #endif
```   

其他：`#error` 产生一个错误

### 大程序
- 中间过程文件：
  - a.c->a.i 预处理
  - a.i->a.s 汇编
  - a.s->a.o 生成目标文件（object）
  - a.o+库->a.out 链接,生成可执行文件
- `gcc -c`只编译，不链接
- `gcc a.c --save-temps`保存中间文件

#### 编译单元
##### 编译单元是每个.c文件
- `a.c-->a.o`的过程称为编译
- 编译器每次编译**只处理一个单元**
  - `gcc a.c b.c` 先`a.c->a.o`,接着`a.o`放一边，`b.c->b.o`,最后再把`a.o b.o`和库链接起来

#### 头文件
在编译`a.c`时不知道`b.c`的内容，假如在`a.c`中使用过`b.c`中定义的函数等，则**编译不能通过**，为解决该问题，须在`a.c`中声明`b.c`的内容，这需要用到头文件
- `include"b.h"`
- 头文件里的文本内容直接被**全部**替换进程序中
##### 标准头文件结构
为避免重复声明，头文件有如下标准结构
```c
#ifndef __A_H__
#define __A_H__
//...
#endif
```

#### 声明
- 头文件中应包含声明而非定义
  - 否则会出现重复的定义
    - 因为定义产生代码，声明不产生
- 声明**不产生代码**，只是让电脑知道，而定义产生代码
- 只有声明可以放在头文件中，定义不行
  - **否则会产生重名的实体**
##### 全局变量的声明
声明前加`extern`
- `extern int a`
- `int a`是定义变量，加上`extern`才是声明
##### 前向声明
一般用在指针，因为你要让编译器知道指针指向什么数据类型，但你不需要具体写出这个类型的内容
```c
struct node;
typedef struct{
  node *head;
}list;
```
在上述例子中，我需要定义`head`指针，则需写下`struct node`的前向声明来告诉编译器这个指针指向`node`这个结构，但我不需要写出`node`具体是什么，因为此时编译器不需要知道

#### `#include`
- `#include`不是用来引入库的,他的文本内容是相应的声明 *类比头文件*
  - 里面只有函数原型，函数代码在另外的地方(即在对应的标准库里，程序知道去哪里找到他)
  - 只是让函数知道

##### `""`与`<>`
- `" "`要求编译器在当前目录寻找文件（即.c文件所在目录）,找不到再去编译器指定的目录去找
- `< >`要求编译器去指定目录找
- 因此编译器知道自己的标准库头文件在哪

