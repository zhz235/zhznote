# graph
## 基本概念
- 由vertices和edge和组成，可以没有edge（散点图），不能没有vertices
- 不允许出现自己指向自己（self loop）和两个顶点间有多条重复的边（multi graph）
- **有向图（directed graph）**：tail->head:$<v_i,v_j>!=<v_j,v_i>$
- **无向图 （undirected graph）**：$(v_i,v_j)==(v_j,v_i)$
- complete graph:有最大数量的边
  - 无向图：顶点数为n,则边数为$n*(n-1)/2$
  - 有向图：顶点数为n,则边数为$n*(n-1)$
- 相邻（adjacent）：顶点间有边相连
  - 对于有向图：$<v_i,v_j>$指$v_i$ is adjacent **to** $v_j$ or $v_j$ is adjacent **from** $v_i$
- 子图 subgraph:顶点是子集，边也是子集
- **path**：从一个点到另一个点经过的路程，长度是边的数量
  - **simple path**：路径上经过的点是不同的
- **DAG**: directed acyclic graph,有向无环图
- 度（degree）：顶点的边的数量
  - 对于有向图，分为in_degree和out_degree
## 图的表示
### 法1：用二维数组矩阵
- 有连接赋值为1，无连接赋值为0
  - 但是空间复杂度过高，因为要$2^n$的空间，远大于实际边数
### 法2：adjacent list邻接表
- 每个结点用链表的形式记录边数
- 对于有向图，记录的是出度或者入度，因此需要维护两个链表或者用一个multilist（同时记录tail和head）
## 最短路径问题
- 计算含权重的最短路径
- 不考虑负边
### 广度优先（breadth first）
从一个顶点开始，先找路径为0的，再找路径为1的（相邻），再找路径为2的（相邻的相邻）······

- **不带权重**
```c
void Unweighted(Table T){
  Queue Q;
  Vertex V,W;
  Enqueue(S,Q);
  while(!isEmpty(Q)){
    V=dequeue(Q);
    for(each W adjacent to V){
      if(T[W].Dist==infinity){
        T[W].Dist=T[V].Dist++;
        T[W].path=V;
        Enqueue(W,Q);
      }
    }
  }
}
```

- **带权重（dijkstra's algorithm）** 
  
1. 把点分为两个集合，一个是已知路径的，记作S，另一个是未知的
2. 对于未知集合中的u,记distance[u]=smallest path to S
3. 选定distance最小的u，把它加入S集合中,并更新（**贪心算法**）
  
```c
 for(;;){
  //一开始所有点距离设为无穷大
  v=smallest unknown distance
  for(each vertex adjacent to v){
    if(!T[w].know){
      if(T[v].dist+cvw<T[w].dist){
        decrease(T[w].dist to t[v].dist+cvw);
        t[w].path=v;
      }
    }
  }
 }
```
### 深度优先 
先一条路走到黑，再逐步后退

## 流量问题
- greedy+undo
- 权重是有理数就可以终止
- O（$E^2$logV）

## 最小生成树
- 包含每个顶点且边的权重和最小
- 存在当且仅当图是连通图
- 可能有多个
### prim algorithm
把点分成两个集合（类似dijkstra算法），从一个顶点出发，每次都找权重最小的相邻顶点加入集合

- 选边
- 先把边按照权重排序，依次选出最小的边，并确保不成环，选出V-1个边为止

## DFS深度优先算法
```c
void DFS(int v){
  visited[v]=true;
  for(each w adjacent to v){
    if(!visited[w])
    DFS[w];
  }
}
void ListComponents(Graph G){
  for(each v in G){
    DFS(v);
    pritnf("\n");//表示有多少个不连通组件
  }
}
```