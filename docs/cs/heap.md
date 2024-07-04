# heap
## 1. min heap
- min heap 是一个完全二叉树并且是一个min tree.
> min tree : 每个父亲节点都比孩子节点小
- min heap 可以用来快速找到最小元素
  - 时间复杂度O(1)
以下介绍min heap的各种操作，maxheap同理可得
### 基本操作
#### 1.1 数组模拟min heap
- 设置array[0]为空头节点

$$ parent[index]=\left\{
\begin{array}{rcl}
i/2 & {i>1}\\
1   &{i=1}\\
\end{array} \right. $$

$$ leftchild[index]=\left\{
\begin{array}{rcl}
2*i & {i*2<=n}\\
None   &{i*2>n}\\
\end{array} \right. $$

$$ rightchild[index]=\left\{
\begin{array}{rcl}
2*i+1 & {i*2+1<=n}\\
None   &{i*2+1>n}\\
\end{array} \right. $$
#### 1.2 定义结构(对完全二叉树用数组表示)：
```c
struct heap{
    int capacity; //最大容量
    int size; //目前大小
    int *array;
}
```
#### 1.3 初始化
```c
heap* creat_heap(int maxsize){
    heap* head=(heap*)malloc(sizeof(heap));
    head->array=(int *)malloc(sizeof(int)*(maxsize+1));
    head->capacity=maxsize;
    head->size=0;
    //把空节点的元素设置为该类型数据的最小元素，以确保任何一个根元素都比其大，如设置为负无穷
    head->array[0]=mindata;
    return head;
}
```
#### 1.4 insertion 
该操作以及接下来的操作的共同思想是：完全二叉树的结构是**固定的**，将要插入或删除的元素放入固定的位置，再比较调整元素间的位置即可
- 常规思路是用交换： 
```c
void insert{heap* head,int x}{
    if(array is full) return;
    head->array[++head->size]=x;
    for(int i=size;head->array[i]<head->array[i/2];i/=2){
        swap(head->array[i],head->array[i/2]);
    }
}
```
- 但是交换太慢了，我们可以用**插入**的思想，**先不把要插入的元素放进去，而是拿在手上，逐步比较并调整相应节点位置后再插入**
```c
void insert{heap* head,int x}{
    if(array is full) return;
    int i;
    for(i=++head->size;head->array[i/2]>x;i/=2){
        head->array[i]=head->array[i/2];
    }
    head->array[i]=x;
}
```
- 时间复杂度O(logn)
#### 1.5 deletemin
- 删除最小元素，即根元素。核心在于要保持树的结构不变，**即把最后一个元素先放到根节点上来，再调整各元素的位置**
- 参照上文的插入思想，我们仍是先把其拿在手上作比较，找到合适的位置再插入
```c
void deletemin(heap* head){
    if(size==0) return;
    int lastelement=head->array[head->size--];
    int i,child;
    for(i=1;i*2<=head->size;i=child){
        child=2*i;
        //找到左右孩子中较小的一个
        if(child<size&&head->array[child]>head->array[child+1]){
            child++;
            }
        if(head->array[child]<lastelement){
            head->array[i]=head->array[child];    
        }else{
            break;
        }
    }
    head->array[i]=lastelement;
}
```

#### 1.6 buildheap
- 先直接读到数组里
- 从最后的非叶子节点开始调顺序（下沉操作）,遍历到1位置
- 时间复杂度为O（n）
  - 最差情况是所有元素都要下称且下称到底，即所有节点高度的和
  - 完美二叉树有$2^{h+1}-1$个节点，所有节点的高度和为$2^{h+1}-1-(h+1)$,其中高度h=logn,故时间复杂度为O(n)
  - 与元素数据大小的先后无关
#### 1.7 其他
- 找第k大的元素：建堆，然后做k次deletemin
- 删除堆中某节点： 先对其做上浮操作，再deletemin