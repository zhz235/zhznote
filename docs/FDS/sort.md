# 排序

## 插入排序

插入排序有 N-1 趟(pass), 对于 $P=1$ 到 $P=N-1$ 趟我们保证位置 0 到位置 $P-1$ 上的元素是已经排序好的，而第 $P$ 趟要做的就是将位置 $P$ 的元素向左移动到它在前 $P+1$ 个元素中的正确位置上。  

``` C
void InsertionSort ( ElementType A[ ], int N ) 
{ 
    int j, P; 
    ElementType  Tmp; 

    for ( P = 1; P < N; P++ ) { 
	Tmp = A[ P ];  /* the next coming card */
	for ( j = P; j > 0 && A[ j - 1 ] > Tmp; j-- ) 
	      A[ j ] = A[ j - 1 ]; 
	      /* shift sorted cards to provide a position 
                       for the new coming card */
	A[ j ] = Tmp;  /* place the new card at the proper position */
      }  /* end for-P-loop */
}
```
### 时间复杂度
* 最佳情况 - 输入数据是已经排好序的，那么运行时间为 $O(N)$  
* 最坏情况 - 输入数据是逆序的，那么运行时间为 $O(N^2)$
- 假设有I个错序对（如（3，2）），则时间复杂度为O(N+I)
  - 每次纠正一组错序对
  - 任意一个数组，平均的逆序对数量是$n*(n-1)/4$
    - 数组一共有n*(n-1)/2个对，每个对有1/2的概率是逆序对
    - 由此得到平均时间效率仍是O($N^2$)

## 希尔排序

希尔排序使用一个 $h_1,h_2,\ldots,h_t$ 的**增量序列**($h_1=1$).  
**$h_k$-sort** 的一般做法是，对于 $h_k,h_k+1,\ldots,N-1$ 中的每一个位置 $i$, 将其元素放到 $i,i-h_k,i-2h_k,\ldots$ 中间的正确位置上。相当于对 $h_k$ 个独立的子数组各进行一次插入排序。$h_k$-sort 之后，对于每一个 $i$ 我们都有 $a_i\leq a_{i+h_k}$, 此时成称为 **$h_k$-sorted**. 

希尔排序的一个重要性质: 一个 **$h_k$-sorted** 的文件（此后将是 **$h_{k-1}$-sorted**）保持他的 **$h_k$-sorted** 性质。  

### 希尔增量序列

$h_t=\lfloor N/2 \rfloor, h_k=\lfloor h_{k+1}/2 \rfloor$(可以有更好的增量序列)

``` C
void Shellsort( ElementType A[ ], int N ) 
{ 
      int  i, j, Increment; 
      ElementType  Tmp; 
      for ( Increment = N / 2; Increment > 0; Increment /= 2 )  
	/*h sequence */
	for ( i = Increment; i < N; i++ ) { /* insertion sort */
	      Tmp = A[ i ]; 
	      for ( j = i; j >= Increment; j - = Increment ) 
		if( Tmp < A[ j - Increment ] ) 
		      A[ j ] = A[ j - Increment ]; 
		else 
		      break; 
		A[ j ] = Tmp; 
	} /* end for-I and for-Increment loops */
}
```

* 最坏情形分析  
使用希尔增量时的希尔排序的最坏情形运行时间为 $\Theta(N^2)$ 

* Hibbard 增量序列  
$h_k= 2^k-1$, 且其最坏情形下运行时间为 $O(N^{3/2})$
## 堆排序
* 以线性时间建一个 Max 堆
* 将堆中最后元素与第一个元素交换，缩减堆的大小并进行下滤。执行 N-1 次 `DeleteMax` 操作  
* 算法终止时，数组按顺序即为从小到大的排序结果

``` C
void Heapsort( ElementType A[ ], int N ) 
{  int i; 
    for ( i = N / 2; i >= 0; i - - ) /* BuildHeap */ 
        PercDown( A, i, N ); 
    for ( i = N - 1; i > 0; i - - ) { 
        Swap( &A[ 0 ], &A[ i ] ); /* DeleteMax */ 
        PercDown( A, 0, i ); 
    } 
}
```

注：这里的堆我们是从 0 开始计数的，因此左儿子应该是 `2*i+1`  

> 对 N 个互异项的随机排列进行堆排序，平均比较次数为 $2N\log N-O(N\log\log N)$

## merge sort 归并搜索
- 基础函数merge，将两个数组排好序发在剩下的数组里

```C
//数组a的长度为len1,数组b的长度为len2，将a,b数组排好序合并入c数组（a,b已排好序）
void merge(int a[],int b[],int c[],int len1,int len2){
  int i=0,j=0,k=0;
  while(i<len1||j<len2){
    if(j==len2||i<len1&&a[i]<b[j]){
      c[k++]=a[i++];
    }else{
      c[k++]=b[j++];
    }
  }
}
```

- 递归写法
```c
void merge(int a[],int begin, int mid,int end ){
  int i=begin,j=mid+1,k=0;
  //定义好len很关键，方便算出n中元素数量
  int len=end-begin+1;
  int b[len];
  while(k<=len-1){
    if(j>end||i<=mid&&a[i]<a[j]){
      b[k++]=a[i++];
    }else{
      b[k++]=a[j++];
    }
  }
  //将b中排好序的拷贝回a
  for(int m=0;m<len;m++){
    a[begin+m]=b[m];
  }
}
void sort(int a[],int begin,int end){
    if(begin<end){
        int mid=(begin+end)/2;
      //这里只能用mid+1,否则会栈溢出  
      //split
        sort(a,begin,mid);
        sort(a,mid+1,end);
      //merge  
        merge(a,begin,mid,end);
      }
}
```
- 迭代写法
    - **思想是从小到大，从两个一组逐步扩大**
```c
void merge(int a[],int begin, int mid,int end ){
  int i=begin,j=mid+1,k=0;
  //定义好len很关键，方便算出n中元素数量
  int len=end-begin+1;
  int b[len];
  while(k<=len-1){
    if(j>end||i<=mid&&a[i]<a[j]){
      b[k++]=a[i++];
    }else{
      b[k++]=a[j++];
    }
  }
  //将b中排好序的拷贝回a
  for(int m=0;m<len;m++){
    a[begin+m]=b[m];
  }
}
int main(){
  for(int k=1;k<len;k*=2){//k表示已经排好的个数
    for(int i=0;i<len;i=i+2*k){
//传送对应的begin mid end 下标，注意不要出现重叠
//min是为了防止下标越界
      merge(a,i,min(i+k-1,len-1),min(i+2*k-1,len-1));
    }
  }
}
```

- 时间复杂度为O(n*logn)

## 快排
快排适用于大量数据，如果数据小于20，一般替换为插入排序

1. 确定哨兵位置，一般情况下设置第一个数，之后一个数和中间数三个数的中位数为哨兵
```c
int mid3(int a[],int left,int right){
//first,a[right]>=a[mid]>=a[left]
  int mid=(right+left)/2;
  if(a[left]>a[mid]){
    swap(a[left],a[mid]);
  }
  if(a[mid]>a[right]){
    swap(a[mid],a[right]);
  }
  if(a[left]>a[right]){
    swap(a[left],a[right]);
  }
//把中间的哨兵放到导数第二的位置
  swap(a[mid],a[right-1]);
///return pivot
  return a[right-1];
}
```
2. 核心算法，确定哨兵后用两个指针i,j，把数组分为比哨兵小和比哨兵大的两部分
```c
void qsort(int a[],int left,int right){
  int i,j;
  if(left+cutoff<right){//这里是保证大量元素用快排，少量用插入
    int pivot=mid3(a,left,right);
    i=left;j=right-1;//注意i,j是比当前要比较的元素少1的
    for(;;){
      while(a[++i]<pivot){}
      while(a[--j]>pivot){}
      if(i<j) swap(a[i],a[j]);
      else break;
    } 
    swap(a[i],a[right-1]);//交换a[i]和哨兵,注意不是j,因为i指向的元素才比哨兵小
    qsort(a,left,i-1);
    qsort(a,i+1,right);   
  }else{
    insertsort(a);
  }
}
```
### 复杂度分析
左右两边快排和partition
$T(N)=T(i)+T(N-i-1)+cN$
- worst case:T(n)=T(n-1)+cN,T(N)=O(n^2)
- best case:T(n)=2T(n/2)+cN,T(n)=O(nlogn)
- average:T(n)=O(nlogn)

## 桶排序(Bucket sort)
- 假设n个同学，总共可能的分数为m，按分数排序
- 要求n>>m
- 建立m个桶，当某个同学的分数是i时，就把他放在count[i]里（可以以链表等形式）
- 时间复杂度为O(m+n)=O(n)


## 基准排序（Radix sort）
### LSD:least significant digit first
1.建立0-9的桶

2.放置：将元素按照最后一个字符放入桶中(**个位开始，依次增加位**)

3.收集并更新桶：按照从0到9的桶序把桶中的元素按照第二字符放在对应的桶中（注意桶中是有序的）

4.重复3直到字符排完
### MSD
1.先按照最重要的关键词放入桶中

2.在每个桶中按照第二重要的关键词排序

3.重复2直到所有的关键词排完