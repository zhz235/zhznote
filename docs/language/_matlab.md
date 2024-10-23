>> 这个课没什么用，笔记随便做的，记的不全
# matlab
!!!abstract
    - 课程：短学期matlab
    - 任课老师：肖俊
    
## 变量命名
- 不能是关键字
  - pi是Π
  - `iskeyword`获取关键字
- 字母开头，由数字字母下划线组成

## 基本语法
### 输入输出
```matlab
a=input('a=');     %a=是输出的提示词
disp('hello world');
```

### 控制语句
1. if语句，缩进表示语句块，注意不要漏掉最后的end
```matlab
if a>b
    disp('1');
elseif a==b
    disp('2');
else
    disp('3');
end
```

2. switch语句
```matlab
switch a
case 1 
   语句段1 
case 2
   语句段2
...
otherwise 
   语句段n 
end 
```

3. for循环
```matlab
for k=1:100
    语句
end
```

## 矩阵

### 创建矩阵
- 直接输入：`A=[1,2;2,3;3,4]`
    - `;`表示换行
- 载入外部文件
  - `load data.txt`
- 内置函数创建
  - zeros(m,n):全0矩阵
  - rand(m.n):随机矩阵
  - eye(m,n):单位矩阵
  - 更多矩阵请查表
### 矩阵寻访
#### 访问单元素
- a[i,j]获取第i行第j列的元素
- 线性下标：对于m*n矩阵，第p行q列的线性下标 $u=(q-1)*m+p$

#### 访问多元素
- H()
### 矩阵拼接

## 函数
```matlab
% s是输出，m是输入
% 可以有多个输入和输出
function[s]=M_Sum(m)
[r c]=size(m);
s=0;
for i=1:r
    for j=1:c
        s=s+m(i,j);
    end;
end;
```

把这个文件以`M_Sum`保存在工作目录下，以后即可调用

## 图像
- 二度图像
- 灰度图像
- RGB图像
- 8位彩色图像（索引图像）
### 抖动
- 抖动算法：用二元打印机（只能打黑或者白）打印多元图像
- 抖动矩阵

## 颜色
### CMY model
打印机用CMY颜色模型

RGB与CMY是互补的，即（量化到0~1之间）：
$[R G B]=[1,1,1]-[C,M,Y]$
$[C,M,Y]=[1,1,1]-[R,G,B]$

### 图像点运算
#### 图像数据类型
- double：[0,1]之间的浮点数
- uint8: [0,255]之间的整数
  - 可以用`im2double`和`im2uint8`实现转化
- rgb图像一般以uint8形式储存，计算的时候可以先转化为double类型，再转回uint8类型:
```matlab
b1=double(a)+0.006*double(a).*(255-double(a));
imshow(uint8(b1));
```
### 直方图（hist）
#### 直方图均衡化

#### 直方图规定化

#### 相似图像识别