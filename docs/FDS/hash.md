# hashing
## hash table
有b个桶（bucket），每个桶放s个物体（slot），桶是行，物体是列
f（x）函数返回的是x处于的桶
T(search)=T(delete)=T(insert)=O(1)
### 基本形式
- n：total number of identifiers in hash table(现在已经放入的物体)
- T: total number of distinct possible values for x
- identifier density：n/T
- loading density： n/sb
- collision：two nonidentical identifiers into the same bucket,i,e,f(i)=f(j)when i!=j;
- overfolw: when a new identifier into a full bucket
  - if s=1,collision=overflow
  - 尽量避免发送collision，不允许发生overflow
### hash function
- f(x)返回x所在的桶（哈希表所在的行）
- easy to compute and minimize collision
- uniform:均匀分布
#### 整数
f(x) = x%tablesize
- 注意tablesize取离size最近的素数
#### 三位字符串
f(x) = $(\Sigma{x[N-i-1]}*32^i)\%tablesize$
- 乘32是为了加速（相当于左移5位）
```c
index hash3(const char* x,int tablesize){
  unsigned int hashval=0;
  while(*x!='\0'){
    hashval=(hashval<<5)+*x++;
  }
  return hashval%tablesize;
}
```
## 解决碰撞问题 
### 1.Separate Chaining
**creat a list of all keys that hash to the same value**

尽量让每个list节点不要太多

- 定义结构

```c
struct node{
  int element;
  struct node* next;
}
struct hashtable{
  int tablesize;
  node** thelists;
}
```

- create an empty table
```c
hashtable creattable(int tablesize){
  hashtable* h;
  int i;
  h=malloc(sizeof(struct hashtable));
  h->tablesize=tablesize; //注意 tablesize 是素数
  h->thelists=malloc(sizeof(node*)*h->tablesize);
  for(i=0;i<h->tablesize;i++){
    h->thelist[i]=malloc(sizeof(struct node));
    h->thelist[i]->next=NULL;
  }
  return h;
}
```
- find a key from hash table  
```c
node* find(int key,hashtable h){
  node* p;
  node* l;
  l=h->thelists[hash(key,h->tablesize)];//hash function
  p=l->next;
  while(p!=NULL&&p->element!=key){
    p=p->next;
  }
  return p;
}
```
### 2.open addressing
**find another empty cell to solve collsin**

用数组实现,探测，开放寻址
```c
index=hash(key);
int i=0;
while(collision){
  index=(hash(key)+f(i))%tablesize;
  i++;
}
```
#### linear probing
f(i)=i
#### quadratic probing
- $f(i)=i^2$
- 理论：对于二次探测的哈希表（大小为素数），至少有一半的元素可以插入进来
  
**find**
```c
position find(int key,hashtable H){
  position currentpos;
  int collisionnum;
  collisionnum=0;//探测次数
  currentpos=Hash(key,H->tablesize);
  while(H->thecells[currentpos].info!=empty&&H->thecells[currentpos].element!=key){
    currentpos+=2*++collisionnum-1;//f(i)=f(i-1)+2i-1
    if(currentpos>=H->tablesize)//等同于模除
      currentpos-=H->tablesize;
  }
  return currentpos;
}
```

**insert**
```c
void insert(int key,hashtable H){
  position pos;
  pos=find(key,H);
  if(H->thecells[pos].info!=legitimate){
    H->thecells[pos].info=legitimate;
    H->thecells[pos].element=key;
    
  }//legitimate表示可用
}
```

### double hash

$f(i)=i*hash_2(X)$  我们在 $X$ 距离 $hash_2(X),2hash_2(X),\ldots$ 等位置进行探测。 常用 $hash_2(X)=R-(X mod R)$, 其中 $R$ 是一个比 $TableSize$ 小的素数。

* 如果正确实现了双重哈希，模拟表明预期的探测数量几乎与随机冲突解决策略相同。
* 二次探测不需要使用第二个哈希函数，因此在实践中可能更简单、更快。

### rehash

对于使用平方探测的开放地址散列法，如果表的元素过多，那么操作的运行时间将开始消耗过长。  

* 建立一个两倍大的表
* 扫描原始散列表
* 利用新的散列函数将元素映射到新的散列值，并插入

$T(N)=O(N)$  

什么时候再散列？

* 表填满一半就再散列
* 当插入失败时
* 当表达到某一个装填因子时进行再散列。  

通常在重哈希之前应该有 $N/2$ 个插入，所以 $O(N)$ 重哈希只会给每个插入增加一个恒定的代价。  
然而，在交互式系统中，不幸的用户的插入导致重新散列，可能会看到速度减慢。