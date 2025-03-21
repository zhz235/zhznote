# 算法
## 回溯法（backtracking）
核心：构建决策树，有错误就回溯，注意剪枝（pruning）

- 八皇后问题
- 收费公路问题（turnpike reconstruction problem）：一共有n(n-1)/2对距离。我们先通过最大距离确定两个端点的距离，然后再依次取剩下的最大的距离来确定新的点的位置（注意这个距离一定是与两个端点的，所以只有两种可能），再通过回溯得到最终结果

代码实现：

```c
bool reconstruct(DistType x[],DistSet D,int N,int left,int right){
//D是给定的距离集合
//x[1]...x[left-1]，x[right+1]...x[N]是已经确定位置的点
    bool Found=false;
    if(is_empty(D)) return True;
    D_max=Find_Max(D);
//case1:x[right]=D_max
    //check函数用来判断这个插入带来的新距离是否与D中距离冲突
    OK=Check(D_max,N,left,right);//pruning
    if(OK){
        x[right]=D_max;
    //更新D中的距离
        for(i=1;i<left;i++) delete(x[right]-x[i],D);
        for(i=right+1;i<=N;i++) delete(x[i]-x[right],D);
    //递归
        Found=reconstruct(x,D,N,left,right+1);
        if(!Found){
            //有冲突，回溯，恢复之前删掉的距离
            for(i=1;i<left;i++) insert(x[right]-x[i],D);
            for(i=right+1;i<=N;i++) insert(x[i]-x[right],D);
        }
    }
//case2:x[left]=x[N]-D_max
    if(!Found){
        OK=check(x[N]-D_max,N,left,right);
        if(OK){
            for(i=1;i<left;i++) delete(x[left]-x[i],D);
            for(i=right+1;i<=N;i++) delete(x[i]-x[left],D);
            Found=reconstruct(x,D,N,left+1,right);
            if(!Found){
            //有冲突，回溯，恢复之前删掉的距离
            for(i=1;i<left;i++) insert(x[left]-x[i],D);
            for(i=right+1;i<=N;i++) insert(x[i]-x[left],D);
            }
        }
    }
    return Found;
}
```

- stick problem
### 示例代码
```c
bool Backtracking(int i){
    bool Found=false;
    if(i>N) return True;
    for(each xi){
        OK=check(xi); //pruning
        if(OK){
            count xi in;
            Found=Backtracking(i+1);
            if(!Found){
                undo(i);
            }
        }
        if(Found) break;
    }
    return Found;
}
```
### 博弈
- 最大最小搜索：当自己的回合时，找收益最大的，当对手回合时，找收益最小的
  - $\alpha-\beta \ pruning$

## 分治法
主定理：
$T(N)=aT(N/b)+O(N^klog^pN)$
$$T(N)=\begin{equation}
    \begin{cases}
    O(N^{log_ba})  & a>b^k \\
    O(N^klog^{p+1}N) & a=b^k \\
    O(N^klog^pN) & a<b^k  
    \end{cases}
\end{equation}$$

## 动态规划

## 近似算法

定义：对于算法A,如果对任意的示例（instance）I 都有：max{A(I)/opt(I),opt(I)/A(I)}<=p(I),则称A是一个p(n)-approximation algorithm

### 装箱问题（Bin packing）
- next fit
  - 一个一个箱子装，如果一个箱子装不下新物体，就把这个箱子封起来然后新开一个箱子

!!! 
    next fit的近似比为2.
    
    证明：设算法用了k个箱子，每个箱子装了$S_k$个物体。则$S_{i}+S_{i-1} \geq 1$,则$opt=总物体数量\geq k-1/2$



- first fit
  - 把物体装到第一个有空位的箱子里，如果没有再新开箱子
- best fit
  - 把物体装到有空位且空余最小的箱子里（充分利用空间），如果没有再新开箱子
- worst fit
  - 把物体装到有空位且空余最大的箱子里，如果没有再新开箱子

> first fit和best fit 近似比为1.7;并且可以通过把输入按从大到小排序来优化。

### 背包问题 Knapsack problem
#### Fractional version
- 每个物体都可以切割成一部分再放入背包
- 算法：通过性价比(v/w)进行**贪心算法**

#### 01背包
在01背包问题下，每个物体只有完全放入和不放入两种选择。

用动态规划算法可以解决：设**f[i][j]表示前i个物体容量为j时的最大价值**，显然对第i个物体，有放和不放两种选择：

**f[ i ][ j ]=max{ f[ i - 1 ][ j ],f[ i - 1 ][ j - w[ i ] ]+ v[ i ] }**

这个算法的时间复杂度是O(nV),V表示物品价值之和。但是这是一个伪多项式时间的，因为输入按照二进制表示后是指数级别的

#### 多项式近似方案
A polynomial-time approximation scheme(PTAS) is a family of algorithm {$A_k$} such that for any k>0,$A_k$ is a (1+k)-approximation algorithm that run in polynomial in n (given k is a constant).

也就是说，可以通过k来调节近似比，然后把k看成一个常数，此时得到的算法的时间复杂度是多项式时间的。

- PTAS:$O(n^{\frac{1}{k}})$
- efficient PTAS(EPTAS): $O(f({\frac{1}{k}}) poly(n))$
- full PTAS(FPTAS): $O(poly({\frac{1}{k}})poly(n))$


### k-center problem