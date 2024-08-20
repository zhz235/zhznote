# 并查集
## 基本结构
- 并查集的核心在于把属于相同集合的元素用一个类似树的结构连接在一起。
  - 这个树是孩子节点指向父亲节点的
  - 用数组表示，数组元素表示父亲节点的索引
  - 根节点的元素是0(也可以设计为小于0)
  - 这样设计树的原因是我们只需要知道元素属于哪个集合，而不需要知道集合元素数量
### union
以下代码中disjset为数组类型
```c
void union(disjset s,int rt1,int rt2){
    s[rt1]=rt2;
}
```
### find
```c
int find(disjset s,int x){
    for(;s[x]!=0;x=s[x]);
    return x;
}
```
### 复杂度分析
并查集的时间复杂度是并操作与查操作加在一起的总的时间复杂度，并的操作永远都是n*O(1)=O(n),因此主要看查的操作。
当是一棵斜树时，查的时间复杂度为$n* O(n)=O(N^2)$   ***即与树的深度相同***
## smart union 
### union by size
- 永远让小树指向大树
- $height（Tree）<=\lfloor logn \rfloor + 1$ 
  - union by size保证了不会出现斜树，又分叉越少树越深，故二叉树为最坏情况
- 时间复杂度nO(logn)
s[0]=-size;

### union by height
浅树指向深树
### path compression
- 路径压缩，对于并查集，分叉越多越好，树越浅越好
- 在找操作的同时，**把每一个沿途的节点都接到根节点上**
递归：
```c
int find(int s[],int x){
    if(s[x]==0) return x;
    else return s[x]=find(s,s[x]);
}
```
迭代：
找根的操作的与之前是一样的，但是我们要用tail记录下x的初位置，然后再做一个循环去把沿途每个节点的父节点改为根节点
```c
int find(int s[],int x){
    int root,tail,lead;
    for(root=x;s[root]!=0;root=s[root]);
    for(tail=x;tail!=root;tail=lead){
        lead=s[tail];
        s[tail]=root;
    }
    return root;
}
```
### union by size和路径压缩结合
时间复杂度为O(n)