# STL
标准模板库。

分类：

- sequential
  - array
    - vector
    - deque
    - forward_list
    - list
- associative,多用红黑树实现
    - set
    - map
    - multiset,multimap
- unordered associative,哈希表实现
- adaptors
    - stack,queue,priority_queue...

## vector
### access

- front返回第一个，back返回第二个
- data用于获取容器底层储存数据的指针

#### at和 [ ]
二者的用法都是传入访问的索引，返回该位置的元素。

不同的是，at会检测索引是否超出容器范围，而operator [ ]不会抛出异常。为了防止越界，多用push_back和resize

```cpp
vector<int> vec={1,2,3};
int element=vec[2];//3
int element=vec.at(2);//3
```
#### 迭代器
类似数组的访问：
```cpp
for(int i=0;i<a.size();i++){}
```

可以用迭代器访问,迭代器类似于指针，首先定义迭代器变量然后使用他，begin(),end()等方法返回的都是迭代器。

```cpp
for(vector<int>::iterator it = a.begin();it<a.end();it++){
    cout<<*it;
}
/* 可以直接把类型简化为auto */
//for(auto it = a.begin();it<a.end();it++)

/* 更简单的： */
//for(int i : a){cout <<i};
```

### capacity
- empty
- size,capacity
- reserve:预分配容器容量，不改变size只改变capacity
    - 如：vec.reserve(10);

### modifiers

```cpp
vector<int> a;

a.push_back(2);

a.insert(a.begin()+4,5,10); //插入的位置，插入数量（可省略），插入元素

a.clear(); //清空容器，即size清零

a.erease(a.begin()+4);  //删除某个位置，后面的元素会自动补上来
```

## map
是键值对，由于底层是红黑树，会自动排好序。

循环遍历：

```cpp
for(const auto& pair: my_map){
  cout<< pair.first << pair.second;
}

for(const auto& [word,cnt]:word_map){};
```

注意` [ ] ` 这个操作符，当我们查找的key值在map里面时，这是一个查找操作；但是如果key值不在map里，会自动将这个key添加到map里并把对应值初始化为0.检查是否包含：map.contains(key)

例如设计一个词频统计器：

```cpp
map<string,int> word_map;
for(const auto& w: {"word1","word2","word1"}){
  word_map[w]++;
}
```

## algorithm
works on a range defined as [first,last).左闭右开。

- for_each,find,count
- sort
- min_element,max_element

## 其他
### copy
copy的使用,注意当没有定义u的长度时，copy传入的第三个参数不能使用u.begin(),而应该用其他迭代器，否则会溢出：

```cpp
vector<int> v={1,2,3,4};
vecotr<int> u;
copy(v.begin(),v.end(),back_inserter(u));  //1,2,3,4
copy(v.begin(),v.end(),front_inserter(u)); //4,3,2,1

copy(u.begin(),u.end(),ostream_iterator<int>(cout,","));

```

### 注意事项
1. erase会返回一个迭代器，应该使用这个返回的迭代器而不能继续用原来的，因为原来的已经erase掉了
2. 用empty（）判断比size()=0判断更快