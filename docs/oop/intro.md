# 基本知识

## 引用

引用可以看成变量的别名，他必须初始化且不能更改绑定的变量。引用会在编译器优化后不占用内存空间。
```cpp
int a=0;
int& ref=a;
ref=10;
```

引用常用于函数传参，比指针更简洁安全：
```cpp
void f(int& a){
    a++;
}
int main(){
    int a=1;
    f(a);
}
```

引用依赖于绑定的对象，必须初始化，不能重新绑定；向参数为引用的函数传参时，不能传表达式；对数组没有引用

## 变量作用域

### global variable
全局变量是所有函数之外的声明，具有全局作用域；

如果没有初始化则默认为0；

**全局变量，静态全局变量，静态局部变量都放在code段**

### local variable
保存在栈上，函数调用完后会清除掉。
### static
static相当于限定使用范围，只有当前编译单元（即当前cpp文件）可以使用。
#### static local variable
仅限声明他的函数内部访问

与local var不同的是他的值是永远保存的，这点更像全局变量。

#### static global variable
内部链接，只能由当前cpp文件访问

### extern
用于声明一个变量或函数，表示其定义在其他文件中，使得编译器不报错

## const
```cpp
//下面这两种既可以指向变量也可以指向常量
const string* cp;      //指针指向的内容不能变
string const* cp;    //也const string等价

//只能指向变量，不能指向常量：
string* p;
string *const p;       //指针不能改，指向的内容可以改
```

指向字符串的指针实际是常量，因为字符串存在data段里是只读的：

```cpp
char* s1="hello";
//等价于 const char* s1
//等价于 const char s1[]
s1[0]='a';//Error!!!

char s2[]="hello"; //字符串被copy到栈上，可以改
s2[0]='a'; //ok
```

永远可以把非常量变量当成常量来对待（即不改变他），但是不能把常量作为变量看，除非强制类型转换（不推荐）。
## 头文件
头文件避免重复，可以用和C语言一样的方法：
```cpp
#ifndef __A_H__
#define __A_H__
//...
#endif
```

更好的方式使用这种编译的控制开关：
```cpp
#pragma once
```

在头文件中定义命名空间,避免不同的命名空间产生冲突：

```cpp
namespace my_namespace{
    void f(){
    //...
    }
}

//在main.cpp中
using my_namespace:: f;
```

## 动态分配内存
### new和delete
通过new动态分配内存，在heap上；

注意区分： `( )` 表示初始化的值，` [ ] ` 表示数量，如：

```cpp
int* p=new int(10);  //分配一个地址并初始化为10
int* r=new int[10]; //分配十个地址（类似数组）
int* q=new int[10]();  //10个初始化为0

delete p;
delete[] r;
delete[] q;
```

注意同一块内存只能delete一次；如果是空指针，可以delete但不会有任何效果。

**注意正确匹配 new/delete 和 new[ ]/delete[ ]**，当分配了多块地址时，一定要用`delete []` 来释放内存。


#### 与malloc和free的区别
当分配内存给类或结构体时，new会首先调用一遍构造函数；当释放内存时，delete也会首先调用析构函数（注意析构函数调用顺序与构造函数相反，是反着来的）。这也是为什么要用`delete[ ]` 的原因.而malloc和free只负责分配指定大小的内存空间，不会去调用构造或者析构函数。

delete是怎么知道要释放多少块内存的呢？在分配内存的头地址之前，系统还会分配一部分内存用来存储分配的个数，delete通过这个知道该释放多少内存以及调用多少次析构函数

