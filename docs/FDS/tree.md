# 树
## 1. 基本概念和性质
- edge=node-1
- 二叉树中，叶节点的个数等于度为0的节点个数加一
  - 二叉树只有度为0，1，2的节点，利用节点与边的关系设方程求解
- 节点的度：子节点的个数
- 树的度：max{degree of the node}
- 叶：没有孩子（度为0）的节点
- length of path:edges的数量
  - 高度height：到叶节点最远的路的长度
  - 深度depth：到根节点的长度
  一颗树转化为二叉树，先序等于先序，原树的后序等于BT的中序
### 1.1 完美二叉树
- 树除了叶节点的每个节点都被填满，即叶节点度为0，其余节点度为2
- 深度为h，则节点数为$2^{h+1}-1$
### 1.2 完全二叉树
- 完美二叉树的前n个节点（按层序遍历）
- 即只有最左边的节点没有填充，且优先向左填充
### 1.3 min tree
每个父亲节点都比孩子节点小
### 1.4 线索二叉树
- 左子树为前驱，右子树为后驱
## 2. 深度优先遍历
### 先序遍历（preorder traversal）
- 顺序：先访问根节点，在依次访问左子树和右子树
- 代码：
```c
preorder(tree_ptr tree)
if(tree){
    visit(tree);
    preorder(tree->left);
    preorder(tree->right);
}
```
### 后续遍历（postorder）
- 顺序：先访问左子节点，再访问右子节点，最后访问根节点
- 代码：
```c
postorder(tree_ptr tree)
if(tree){
    postorder(tree->left);
    postorder(tree->right);
    visit(tree);
}
```
### 中序遍历（inorder）
- 顺序：先访问左子节点，再访问根节点，最后访问右子节点
- 代码：
```c
inorder(tree_ptr tree)
if(tree){
    inorder(tree->left);
    visit(tree);
    inorder(tree->right);
}
```
## 3. 广度优先遍历： 层序遍历
- 顺序：一层一层，从左到右
- 代码：
```c
void levelorder(tree_ptr tree)
    enqueue(tree);
    while(queue is not empty){
        visit(T=dequeue());
        for(each child c of T)
            enqueue(c);
    }
```

## 4. 中序和后续（前序）遍历生成二叉树
核心思想是通过后序（前序）找到根节点，再利用中序找到根节点左右的子树
代码实现：
```c
int findtree(int inorder[],int size,int k){
    for(int i=0;i<size;i++){
        if(inorder[i]==k){
            return i;
        }
    }
}
tree buildtree_in_postorder(int inorder[],int n1,int postorder[],int n2){
    if(n1==0){
        return NULL;
    }
    tree *t=(tree*)malloc(sizeof(tree));
    t->data=postorder[n2-1];
    int root=findtree(inorder,n1,t->data);
    t->left=buildtree(inorder,root,postorder,root);
    t->right=buildtree(inorder+root+1,n1-root-1,postorder+root,n2-root-1);
    return t;
}
tree buildtree_in_preorder(int inorder[],int n1,int preorder[],int n2){
    if(n1==0){
        return NULL;
    }
    tree *t=(tree*)malloc(sizeof(tree));
    t->data=preorder[0];
    int root=findtree(inorder,n1,t->data);
    t->left=buildtree(inorder,root,preorder+1,root);
    t->right=buildtree(inorder+root+1,n1-root-1,preorder+root,n2-root-1);
    return t;
}
```
## 5. binary searching tree
### 5.1 find
循环版本：
```c
tree* find(int x,tree* tree);
while(tree){
    if(tree->data==x){
        return tree;
    }else if(tree->data<x){
        tree=tree->right;
    }else{
        tree=tree->left;
    }
}
return NULL;
```
递归版本：
```c
tree* find(int x,tree* tree){
    if(tree==NULL){
        return NULL;
    }
    if(tree->data==x){
        return tree;
    }else if(tree->data<x){
        return find(x,tree->right);
    }else{
        return find(x,tree->left);
    }
}
```

- 时间复杂度：O(depth)
#### findmin and findmax
- 最小的是左子树的左一个叶节点，最大的是右子树最右的一个叶节点
```c
tree* findmin(tree* t){
    if(t){
        while(t->left){//终止条件是t的左节点已经走完，此时找到最左边的一个节点
            t=t->left;
        }
    }
    return t;
}
```
### 5.2 insert
```c
tree* insert(int x,tree* t){
    if(t==NULL){
        t=(tree*)malloc(sizeof(tree));
        t->data=x;
        t->left=t->right=NULL;
    }else{
        if(x>t->data){
            t->right=insert(x,t->right);
        }else{
            t->left=insert(x,t->left);
        }
    return t;
    }

}
```
### 5.3 delete
```c
tree* delete(int x,tree* t){
    if(t==NULL){
        printf("error");
    }else if(t->data>x){
        t->left=delete(x,t->left);
    }else if(t->data<x){
        t->right=delete(x,t->right);
    }else{
        if(t->right&&t->left){
            tree* tempcell=findmin(t->right);
            t->data=tempcell->data;
            t->right=delete(t->data,t->right);
        }else{
            tree *tempcell=t;
            if(t->right==NULL){
                t=t->left;
            }else{
                t=t->right;
            }
            free(tempcell);
        }
    }
    return t;
}
``` 

- time complexity=O(depth)
