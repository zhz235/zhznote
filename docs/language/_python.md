# python
## 变量和数据类型
变量的命名规则和C语言是一样的，即由数字下划线和字母组成，不能用数字开头。
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

### 列表（List）
列表是由 `[ ]` 表示，并用逗号分隔其中的元素。列表下标由0开始，索引方式与C语言相同。不同的是，可以用 **-1** 访问最后一个元素，以此类推，-2访问倒数第二个元素……

**列表插入元素：**
```python
list=['a','b']
list.append('c')   #在末尾插入
list.insert(1,'c') #在索引为1处插入，list=['a','c','b']
```

**列表删除操作：**
`del` 删除指定索引的元素：
```python
list=['a','b','c']
del list[0]   # 现在list=['b','c']
```

`pop` 弹出指定索引元素，相当于删除并使用。注意，默认弹出栈顶元素，即列表末尾元素。
```python
list=['a','b','c']
x=list.pop()   # x='c',list=['a','b']
list=['a','b','c']
y=list.pop(1)  # y='b',list=['a','c']
```

`remove` 用来移除已知大小不知索引的元素。注意，remove只会移除最先发现的值，如果有重复的，则不会移除
```python
list=['b','a','b']
list.remove('b')   # 现在list=['a','b']
```