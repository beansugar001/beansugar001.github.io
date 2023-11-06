---
layout:     post   				    # 使用的布局（不需要改）
title:      Data Structure			# 标题 
subtitle:   Tree  #副标题
date:       2023-10-29 				# 时间
author:     beansugar 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - Data Structure
---

记录一下数据结构的学习过程~~前面简单的到时候再写

# 树的定义

**树(Tree)**:n(n>=0)个结点构成的有限集合。  
当n==0时称为**空树**  
当n>0时,具备一下性质

* 树中有一个称为“**根（Root）**”的特殊结点，用r表示
* 其余结点可分为m（m>0）个**互不相交的**有限集T1,T2...Tm,其中每个集合又称为**子树(SubTree)**
* 除了根节点外，每个结点有且仅有一个父节点
* 一颗N结点的树有N-1条边

### 程序中的定义方法

```c
// 定义二叉树节点结构
typedef struct TreeNode {
    int data;
    struct TreeNode* left;//left结点也拥有data,left,right 结构，其中right和left为指针
    struct TreeNode* right;
} TreeNode;
```

# 树的基本术语

1. 结点的度(Degree):结点子树的个数
2. 树的度：树的所有结点中最大的度数（结点拥有的最多子树）
3. 叶节点（Leaf）：度为0的结点，也称叶端点
4. 父结点（Parent）：具有子树的结点是其子树的根节点的父结点
5. 子结点（Child）：父结点的下一个结点
6. 兄弟结点（Sibling）：具有同一父结点的几个子结点
7. 路径和路径长度：从结点N1到Nk的**路径**为一个结点序列。路径所包含的**边数**为**路径长度**
8. 祖先结点（Ancestor）：沿**树根到某一结点路径上的所有结点**都是这个结点的祖先结点
9. 子孙结点（Descendant）：某一结点**子树中的所有结点**是这个结点的子孙节点
10. 结点的层次（Level）：规定根结点在1层，其他任意结点的层数是其父结点的层数加1
11. 树的深度（Depth）：树中所有结点的**最大层次**是这棵树的深度

# 二叉树

定义：有穷结点集合，可以为空，分为左子树和右子树

1. 特殊二叉树

斜二叉树（Skewed Binary Tree）  
完美二叉树
满二叉树
**完全二叉树**：有n个结点的二叉树，对书中结点按自上而下、从左到右顺序进行编号，编号为i结点与满二叉树中编号为i结点在二叉树中位置相同

2. 几个重要性质

* 一个二叉树第i层的最大结点数为：2^(i-1),(i>1)
* 深度为k的二叉树有最大结点总数为：2^k -1 ,k>=1
* 对任意非空二叉树T，若n0代表叶节点的个数为0，n2代表度为2的非叶节点个数，那么两者满足关系n0=n2+1

3. 二叉树的储存结构

**顺序存储结构**

* 非根结点的父结点的序号是[i\1]
* 结点的左孩子的序号是2i（若2i+1<=n,则没有左孩子）
* 结点的右孩子的序号是2i+1(若2i+1<=n,则没有右孩子)

#### 创建二叉树的结点

```c
//一般情况
TreeNode* createNode(int data) {
    TreeNode* newNode = (TreeNode*)malloc(sizeof(TreeNode));
    newNode->data = data;
    newNode->left = NULL;
    newNode->right = NULL;
    return newNode;
}
```

```c
//后序和中序生成树
TreeNode* BuildTree(int* lst, int* mid, int n) {//其中lst和mid为数组，n为数组大小
    int i;
    if (n <= 0) return NULL;
    TreeNode* b = (TreeNode*)malloc(sizeof(TreeNode));
    b->data = lst[n - 1];//后序遍历的最后一个结点为根结点
    for (i = 0; i < n; i++) {
        if (mid[i] == lst[n - 1]) break;//找到中序遍历中根结点的位置，将其分为前后两端
    }
    int k = i;//前段的长度
    int l = n - i - 1;//后段的长度
    b->left = BuildTree(lst, mid, k);
    b->right = BuildTree(lst + k, mid + i + 1, l);//将分出来的两端进行比较，重复递归操作
    return b;
}
```

```c
//先序和中序,树为char类型生成树
BTNode* CreateBTree(char *pre, char *in, int n)
{
    int k;
    char *p;
    if (n <= 0)
        return NULL;
    BTNode *b = (BTNode*)malloc(sizeof(BTNode));
    b->data = *pre;
    for (p = in; p < in + n; ++p)
        if (*p == *pre)
            break;
    k = p-in;
    b->lchild = CreateBTree(pre+1, in, k);
    b->rchild = CreateBTree(pre+k+1, p+1, n-k-1);
    return b;
}
```

#### 二叉树的遍历

###### **1.先序遍历**

过程：1.访问根节点2.先序遍历左子树3.先序遍历右子树

```c
void PreOrderTreversal(BinTree BT)
{
    if(BT){
        printf("%d",BT->Data);
        PreOrderTreversal(BT->Left);
        PreOrderTreversal(BT->Right);
    }
}

```

**先序非递归**

```c
void IsOrderTreversal(BinTree BT)
{
    BinTree T=BT;/*创建并初始化堆栈S*/
    Stack S = CreatStack(Maxsize);
    while( T||!IsEmpty(S) ){
        while(T){/*一直向左并将沿途结点压入堆栈*/
            printf("%5d",T->Data);/*（访问）打印结点*/
            Push(S,T);
            T=T->Left;
        }
        if(!IsEmpty(s)){
            T = Pop(S);/*结点弹出堆栈*/
            T=T->Right;/*转向右子树*/
        }
}
```


###### **2.中序遍历**

过程：1.中序遍历左子树2.访问根节点3.中序遍历右子树

**数组递归**

```c
void PreOrderTreversal(BinTree BT)
{
    if(BT){
        PreOrderTreversal(BT->Left);
        printf("%d",BT->Data);
        PreOrderTreversal(BT->Right);
    }
}

```

**堆栈非递归**

```c
void IsOrderTreversal(BinTree BT)
{
    BinTree T=BT;/*创建并初始化堆栈S*/
    Stack S = CreatStack(Maxsize);
    while( T||!IsEmpty(S) ){
        while(T){/*一直向左并将沿途结点压入堆栈*/
            Push(S,T);
            T=T->Left;
        }
        if(!IsEmpty(s)){
            T = Pop(S);/*结点弹出堆栈*/
            printf("%5d",T->Data);/*（访问）打印结点*/
            T=T->Right;/*转向右子树*/
        }
}
```

###### **3.后序遍历**

过程：1.后序遍历左子树2.后序遍历右子树3.访问根节点

```c
void PreOrderTreversal(BinTree BT)
{
    if(BT){
        PreOrderTreversal(BT->Left);
        PreOrderTreversal(BT->Right);
        printf("%d",BT->Data);
    }
}

```

##### **层序遍历**

1. 数组实现



2. 队列实现

算法思想：借助一个队列，根树进队；队不为空时循环“从队列中出一个树p，访问该树根结点；*若它有左子树，左子树进队；若它有右子树，右子树进队。”

```c

#include<stdio.h>
#include<stdlib.h>
#include<malloc.h>
typedef struct TreeNode{
	TreeNode *left;
	TreeNode *right;
	int data;
}TreeNode,*pTreeNode;
 
typedef struct QueueNode{
	pTreeNode data;
	QueueNode *next;
}QueueNode,*pQueueNode;
 
typedef struct Queue{
	pQueueNode front;
	pQueueNode rear;
}Queue,*pQueue;
 
void create(pTreeNode *t){
	int ch;
	scanf_s("%d",&ch);
	if(ch==-1){
		(*t)=NULL;
		return;
	}else{
		(*t)=(pTreeNode)malloc(sizeof(TreeNode));
		(*t)->data=ch;
		printf("请输入%d的左节点数据:",ch);
		create(&((*t)->left));
		printf("请输入%d的右节点数据:",ch);
		create(&((*t)->right));
	}
}
 
pQueue init(pQueue pq){
	pq->front=(pQueueNode)malloc(sizeof(QueueNode));
	pq->front->next=NULL;
	pq->rear=pq->front;
	return pq;
}
 
void enqueue(pQueue pq,pTreeNode t){
	pQueueNode pNew=(pQueueNode)malloc(sizeof(QueueNode));
	pNew->data=t;
	pNew->next=NULL;
	pq->rear->next=pNew;
	pq->rear=pNew;
}
 
pTreeNode dequeue(pQueue pq){
	pQueueNode pTemp=(pQueueNode)malloc(sizeof(QueueNode));
	pTemp=pq->front->next;
	if(pTemp->next==NULL){
		pq->rear=pq->front;
	}else{
		pq->front->next=pTemp->next;
	}
	pTreeNode x=pTemp->data;
	free(pTemp);
	return x;
}
 
void LevelOrderBinaryTree(pTreeNode t){
	pQueue pq=(pQueue)malloc(sizeof(Queue));
	pq=init(pq);
	enqueue(pq,t);
	while(pq->rear!=pq->front){
		pTreeNode x=dequeue(pq);
		printf("%d ",x->data);
		if(x->left){
			enqueue(pq,x->left);
		}
		if(x->right){
			enqueue(pq,x->right);
		}
	}
}
void main(){
	pTreeNode t;
	printf("请输入第一个节点数据,-1代表没数据:");
	create(&t);
	system("pause");
	printf("层序遍历如下:");
	LevelOrderBinaryTree(t);
	system("pause");
}
```

### **完全二叉树后序遍历倒推输出层序遍历**

```c
#include<stdio.h>//假设递归范围[l, r],r这个位置就是该子树的根,左子树就是[l + size / 2 - 1]
#define MAX 100
int a[MAX],tree[MAX];
int se=0,n;//巧妙运用se来作为数组a的下标使其与层序遍历的下标对应
void operate(int h)
{
    if(h>n)return ;
    int L=h*2;
    int R=h*2+1;
    operate(L);operate(R);
    tree[h]=a[++se];
}
int main()
{
    scanf("%d",&n);
    for(int i=1;i<=n;i++)scanf("%d",&a[i]);
    operate(1);
    for(int i=1;i<=n;i++)i==n?printf("%d",tree[i]):printf("%d ",tree[i]);
    return 0;
}
```


