# python基础
## 变量和数据类型
变量的命名规则和C语言是一样的，即由数字下划线和字母组成，不能用数字开头。变量命名习惯性用小写。

- **可变对象：** 可以原地修改对象的值，近似于C语言的指针。比如作为函数参数传入函数并在函数内改变其值后，它的值会永久性修改。**包括列表，字典，集合等**
- **不可变对象：** 比如传入函数只是传入形参，不改变其值。**包括数字，元组等**
### 字符串
字符串是由单引号或者双引号括起来的，这种灵活性可以使得字符串中包含引号和撇号。
#### 方法
可以使用 **方法(method)** 修改字符串：
```python
name="hello world"
print(name.title())   #首字母大写(Hello World)
print(name.upper())   #全部大写(HELLO WORLD)
print(name.lower())   #全部小写(hello world)
print(name.rstrip())  #消除字符串末尾空格
print(name.lstrip())  #消除字符串开头空格
print(name.strip())   #消除字符串两端空格

```

name后的`.`表示对变量name执行操作，后面的括号提供格外的信息，没有格外信息即为空括号

#### 字符串拼接
可以使用`+`来进行字符串拼接：
```python
message="hello," + " " + "world" + "!"
print(message)
#输出：hello, world!
```

#### 空白
在编程中，空白泛指**任何非打印字符，如空格、制表符和换行符**。你可使用空白来组织输出，以使其更易读

- 换行 `\n` ，制表 `\t`
- 可以使用 **方法** 来消除空白

### 数字
- 数字类型包括整数和浮点数
- `\` 得到的结果是浮点数，`\\` 与C语言中的`\`相同
- `**` 表示乘方
- `str` 可以将数字转化为字符串，如 `str(5+3)` 得到 “8”

### 列表（list）
列表是由 `[ ]` 表示，并用逗号分隔其中的元素。列表下标由0开始，索引方式与C语言相同。不同的是，可以用 **-1** 访问最后一个元素，以此类推，-2访问倒数第二个元素……

列表中的元素可以是数字，字符串等任意类型

**列表插入元素：**
```python
_list=['a','b']
_list.append('c')   #在末尾插入
_list.insert(1,'c') #在索引为1处插入，_list=['a','c','b']
```

**列表删除操作：**
`del` 删除指定索引的元素：
```python
_list=['a','b','c']
del _list[0]   # 现在_list=['b','c']
```

`pop` 弹出指定索引元素，相当于删除并使用。注意，默认弹出栈顶元素，即列表末尾元素。
```python
_list=['a','b','c']
x=_list.pop()   # x='c',_list=['a','b']
_list=['a','b','c']
y=_list.pop(1)  # y='b',_list=['a','c']
```

`remove` 用来移除已知大小不知索引的元素。注意，remove只会移除最先发现的值，如果有重复的，则不会移除
```python
_list=['b','a','b']
_list.remove('b')   # 现在_list=['a','b']
```

#### 列表排序
**使用sort进行永久排序：**

sort对列表的修改是永久性的。如果列表同时含有字符串和数字等类型，需定义排序规则（如都转化为字符串）再排序
```python
_list.sort()  #正序
_list.sort(reverse=True)  #逆序
_list.sort(key=str)  #将所有元素转化为字符串排序
```

**使用sorted进行临时排序**

sorted不会修改原列表，只是得到对应的结果
```python
sorted(_list)
```

**其他**
```python
_list.reverse() #翻转列表
len(_list)  #获取长度
```

#### 列表遍历
采用for循环遍历列表：
```python
magicians = ['alice', 'david', 'carolina'] 
for magician in magicians: 
    print(magician) 
```

for循环每次从列表 `magicians` 中取出一个元素，并将其储存在变量   `magician` 中。注意不要遗漏for循环后的**冒号**。

#### range
range是左闭右开的，可以指定步长:
```python
for value in range(0,6,2):
    print(value)
#输出：0 2 4
```

`list()` 可以把数字转化为列表，可以用range创建数值列表：
```python
number=list(range(1,4))
#number=[1,2,3]
```

**列表解析**：列表解析将for循环和创建新元素的代码合成一行，注意此时for循环后没有冒号，例如：
```python
squares=[value**2 for value in range(1,4)]
# square=[1,4,9]
```

#### 切片
切片是处理列表中的部分元素，你可以指定起始和末尾索引（**同样是左闭右开**）。如果没有指定起始索引，默认从第一个开始；若没有指定结束索引，默认到最后一个结束。
```python
_list=[1,2,3]
print(_list[:2])  # 1 2
print(_list[-2:]) # 2 3
```

用于列表是可改变对象（类似指针），如果想要创建一个副本，则可以同时省略开头末尾来复制列表：
```python
a=list(range(1,4))
a1=a[:] #a1是另一个副本
a2=a #a2和a都是指向原列表的，修改a2也会改变a
```

### 元组（Tuple）
使用 `( )` 表示元组，其他特点与列表类似。不同的是，元组的元素**不可修改**

### 字典（Dictionary）
字典是一系列 **键(key)-值(value)** 对。每个键都与一个值相关联，你可以使用键来访问与之相关联的值。与键相关联的值可以是数字、字符串、列表乃至字典。事实上，可将任何Python对象用作字典中的值。字典用 `{ }` 表示,对应的key与value之间用冒号，如：
```python
alien={'color':'green'}
print(alien['color'])  # 输出：green
# 添加值
alien['number']=1      # alien={'color':'green','number':1}
# 删除值
del alien['color']     # alien={'number':1}
```

#### 遍历字典
**遍历键值对:**

使用方法items，会返回一个键值对的列表：
```python
# k is the key,v is the value
for k,v in dictionary.items():
    ······
```

**遍历键:**

使用方法keys，返回键的列表，如果不使用任何方法，也是默认为键的列表
```python
# k is the key
for k in dictionary.keys():
    ······
# 等价：
for k in dictionary:
    ······
```

**遍历值:**

使用方法values，返回值的列表。由于值是有可能重复的，可以采用set来剔除重复性。集合类似列表，但每个元素独一无二。
```python
# v is the value
for v in dictionary.values():
    ······
# 剔除重复项
for v in set(dictionary.values()):
    ······
```

值得注意的是，**字典获得元素的值的顺序是不可预测的**。如果想让遍历的值有顺序，可以用sorted：
```python
# v is the value
for v in sorted(dictionary.values()):
    ······
```



## 控制流语句
### if语句
python中的布尔运算符：and,or,not，对应C语言中&&，||和！。一般结构为 `if-elif-else` 注意不要漏掉if,elif,else后的**冒号**

判断元素在列表中用 `in` ;不在列表中用 `not in` 。 **空列表和空字符串也是False**,由于空列表也是False,所以可以用if语句判断列表是否为空.
```python
number=[1,2,3,4]
if number:
    if 1 in number and 2 not in number:
        print('1')
    else:
        print('0')
```

### while语句
可以用break，continue等控制循环

## 输入
`input` 得到给出提示信息,并将用户输入作为**字符串**返回：
```python
name=input('please input your name')
```

如果需要把输入作为数字使用，可以使用`int()` 以及`float()` 等进行转化

## 函数
函数通过 `def` 来定义，传参时，可以用位置实参（同C语言）和关键字实参的方式（传参顺序可改变），参数可设置默认值，但未设默认值的形参必须放前面。函数可以没有return，如果没有return默认返回*None*
```python
def pet(name,itstype='dog'):
    ······
"""以下输出相同"""
pet('jack')  #第二个参数使用默认值
pet('jack','dog')  
pet(itstype='dog',name='jack') #关键字传参，可换顺序
```

如果想要函数传入的列表不被修改，可以传入副本：
```python
function(list_name(:))
```

**传入任意数量的参数：**

使用` *argu_name ` ,会创建一个**元组**，接收任意数量的参数,如：
```python
def function(*number): # 元组
    ······
function('1','2','3')
```

也可以使用 `**argu_name` ,会创建**字典**，可以用来传入关键字：
```python
def function(**age): # 字典
    for k,v in age.items():
        print(k) #关键字
        print(v) #值
function(jack='1',mary='2')
```

### 导入模块
**导入整个模块:** 

用import导入模块，然后用`module_name.function_name()` 的形式调用模块
**导入特定函数:** 

导入模块中的特定函数，这样就可以直接调用函数，
> `from module_name inport function_name`

**as指定别名：** 

如果导入函数名太长或者与现有函数冲突，可以给函数指定别名。
> `from module_name import function as f`

同样可以给模块别名以简化。
> `import module_name as m`

**导入所有函数：**

> `from module_name import *`

## 类
### 类的创建
类定义了对象的属性和方法（*类中的函数即为方法*）。类的名称通常以大写字母开头。
```python
class Myclass:
    def __init__(self,name):
        self.name=name
    def itsname(self):  
        print(self.name)
    ······
```
其中，`__init__` 方法是一个默认的方法，会在创建类时自动调用；`self` 参数是一个指向实例本身的引用，让实例能够访问类中的属性和方法，self会自动传递，因此我们不需要传递它，但注意类中的方法必须传入self参数。

###  类的使用
init方法虽然没有显示的return，但会自动返回这个类的实例，我们将这个返回示例存在 `my_class` 这个变量中，然后可以句点来获取这个实例在类中的属性和方法：
```python
my_class=Myclass('jack')  #传给init的参数
print(my_class.name)  #获取属性name
my_class.itsname()  #调用方法itsname
```

想要修改属性的值，可以直接通过实例（即句点表示）来修改，也可以在类中设置修改的方法

### 继承
一个类继承另一个类时，它将自动获得另一个类的所有属性和方法；原有的类称为父类，而新类称为子类。子类继承了其父类的所有属性和方法，同时还可以定义自己的属性和方法以及修改父类。定义子类时需在括号中指出父类，注意子类的方法`__init__` 通过 `super()` 与父类联系。例如：
```python
'''父类'''
class Restaurant:
    def __init__(self,restaurant_name):
        self.restaurant_name=restaurant_name
    def show(self):
        print('未修改')
'''子类'''
class IceCreamStand(Restaurant):
#注意__init__的区别
    def __init__(self,restaurant_name):
        super().__init__(restaurant_name)
        self.flavors='good'
#修改父类的方法
    def show(self):
        print('修改')
```

> 如果子类添加的细节过多，可以把他们封装在新的类中，并将这个类的实例用作该子类的一个属性

## 文件
**读：** 关键字with在不再需要访问文件后将其关闭。read()读取这个文件的全部内容，并将其作为一个**字符串**存储在变量中。
```python
with open('filepath') as file_object:
    contents=file_object.read()
    lines=file_object.readline() #逐行读取
```

**写：** 以字符串形式写入，如果写入的文件不存在，函数open()将自动创建它.

使用关键字w,写入的内容会覆盖原内容；使用关键字a,写入的内容加到原内容后面。
```python
with open(filepath, 'w') as file_object: 
     file_object.write("python\n") 
```

## 异常
每当程序出错时，python都会创建一个异常对象。如果你未对异常进行处理，程序将停止，并显示一个traceback，其中包含有关异常的报告。使用 `try-except` 代码块可以让Python执行指定的操作，即便出现异常，程序也将继续运行，同时显示你编写的友好的错误消息，而不是令用户迷惑的报错。以下是处理除数为0的错误的示例：
```python
try: 
    answer = x/y 
except ZeroDivisionError: 
    print("You can't divide by 0!") 
    # 也可以使用pass让程序不显示任何报错信息而继续执行
else: 
    print(answer) 
```


