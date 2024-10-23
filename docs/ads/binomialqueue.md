# week 5
## Binomial Queue 二项队列
### 定义
1. 二项队列是一个森林（即很多树组成），每棵树具有堆的性质（下文以最小堆为例）
2. 高度为0的二项树只有一个节点
3. 高度为k的二项树$B_k$是把一个$B_{k-1}$作为另一颗$B_{k-1}$的根节点的子树

### 性质
基本性质：

- $B_k$ 根节点有k个孩子，分别是$B_0,B_1,...B_{k-1}$
- $B_k$有$2^k$个节点
- $B_k$深度为d的节点数量为$C_k^d$

由于任何数字都可以被**二进制**表示，因此知道节点数就可以**唯一**确认二项队列

### Findmin
直接遍历所有的根节点就可以找到最小的值。由于最多logN个节点，所以时间复杂度T=O(logN)

但是这样的操作有些冗余，一个优化是一直记录最小的值并维护，所以时间复杂度为O(1)。**考试写这个**

### Merge
类似**二进制的加法**，如果只有一个队列有$B_k$,就直接保留；两个都有$B_k$，就合并；加上进位有三个$B_k$，就合并两个留一个。

合并两个二项队列是O(1),最多合并O(logN)次（要求二项队列按高度排序），故时间复杂度T=O(logN)。

### Insert
看作特殊的merge操作。

#### 均摊分析
从一个空的二项队列开始插入n个元素是O(n)的复杂度

**聚合法：**

插入有两种操作，一种是step:改变根节点；一种是link：合并操作。可以发现每次都有1次step，所以总step=N。

合并操作可以看成二进制加法，link=1对应01->10,link=2对应011->100,以此类推，总link=$\frac{1}{4}+\frac{1}{8}*2+\frac{1}{16}*3···=O(N)$

因此从0开始插入，每次插入的均摊复杂度为O(1)

**势能法：**

注意到每次link树的数量减少1，每次step树的数量增加1，假设进行c次操作，那么有1次step和c-1次link,因此增加的树的数量为2-c(可能为负数)。

那么定义势能函数$D_i$为树的数量，那么$D_0=0$且$D_i>=0$。有：$\hat{c_i}=c_i+D_i-D_{i-1}=c_i+2-c_i=2$,有总均摊成本=O(2N)=O(N),得证。
### Deletemin
1. Findmin,T=O(logN)
2. 删除最小节点，把所有孩子节点看作一个新的二项队列，T=O(1)
3. merge,T=O(logN)

因此时间复杂度为O(logN)

### 代码实现
采用left-child-next-sibling with linked lists的结构，其中子树按高度**降序**排列（之所以降序是因为合并操作时，是把一个$B_k$插入成另一个$B_k$的子树，降序排序只要O(1)时间就可以找到位置）

结构定义：
```c
typedef struct BinNode{
    int data;
    struct BinNode* leftchild;
    struct BinNode* NextSibling;
}Bintree;

struct BinQueue{
    int currentsize;
    Bintree* tree[Maxsize];
}
```

合并操作(equal size)：

```c
Bintree* combine(Bintree* T1,Bintree* T2){
    //默认T1根节点小于T2
    if(T1->data>T2->data){
        return Bintree(T2,T1);
    }
    T2->nextsibling=T1->leftchild;
    T1->leftchild=T2;
    return T1;
}
```

merge操作:

```c
BinQueue merge(BinQueue H1,BinQueue H2){
    Bintree* T1,T2,carry=NULL;
    H1->currentsize+=H2->currentsize;//默认返回H2
    for(int i=0,j=1;j<=H1->currentsize;i++,j*=2){
        T1=H1->tree[i],T2=H2->tree[i];
        //!!是把表达式转化为布尔值
        switch(4*!!carry+2*!!T2+!!T1){
            case 0:/*000*/  break;
            case 1:/*001*/  break;
            case 2:/*010*/  H1->tree[i]=T2;
                            H2->tree[i]=NULL;
                            break;
            case 3:/*011*/  carry=combine(T1,T2);
                            H1-tree[i]=H2->tree[i]=NULL;
                            break;
            case 4:/*100*/  H1-tree[i]=carry;
                            carry=NULL;
                            break;
            case 5:/*101*/  carry=combine(T1,carry);
                            H1->tree[i]=NULL;
                            break;
            case 6:/*110*/  carry=combine(carry,T2);
                            H2->tree[i]=NULL;
                            carry=NULL;
                            break;
            case 7:/*111*/  H1->tree[i]=carry;
                            carry=combine(T1,T2);
                            H2->tree[i]=NULL;
                            break;
        }
    }
    return H1;
}
```

Deletemin：

```c
int deletemin(BinQueue H){
    Binqueue deletedqueue;
    Bintree* deletetree,oldroot;
    int min=infinity;
    int i,j,mintree; //mintree is the index of the minimum item
//step1:find the minimum element
    for(i=0;i<Maxsize;i++){
        if(H->tree[i]&&H->tree[i]->data<min){
            min=H->tree[i]->data;
            mintree=i;
        }
    }
    deletetree=H->tree[i];
//step2:remove the mintree from H
    H->tree[i]=NULL;
//step3.1:remove the root
    oldroot=deletetree;
    deletetree=deletetree->leftchild;
    free(oldroot);
//step3.2 creat H'
    for(j=mintree-1;j>=0;j--){
        deletedqueue->tree[j]=deletetree;
        deletetree=deletetree->nextsibling;
        deletequeue->tree[j]->nextsibling=NULL;
    }
    H->currentsize-=deletequeue->currentsize+1;
//step4:merge
    H=merge(H,deletequeue);
    return minitem;
}
```