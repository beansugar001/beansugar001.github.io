---
layout:     post   				    # 使用的布局（不需要改）
title:      Data Structure of evel-order Traversal 				# 标题 
subtitle:   evel-order Traversal #副标题
date:       2023-10-30 				# 时间
author:     beansugar 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - Data Structure
---

## 队列实现

#### 思路

借助一个队列，根树进队。若队列不为空，从front出队，若有左子树，左子树进队，若有右子树，右子树进队，进行递归操作。

#### 前期准备

```c
#include<stdio.h>
#include<stdlib.h>
#include<malloc.h>

//定义树的结构体
typedef struct TreeNode{
    int data;
    TreeNode *left;
    TreeNode *right;
}TreeNode,*pTreeNode;/*TreeNode的作用是给 struct TreeNode 结构体类型一个别名。
这允许你在代码中使用 TreeNode 作为结构体类型的名称，而不必每次都使用 struct TreeNode 来引用它。
*pTreeNode它是一个指向 TreeNode 结构体的指针类型。这意味着你可以使用 pTreeNode 来声明指向 TreeNode 结构体的指针变量，
如pTreeNode myTreeNode;声明一个指向 TreeNode 的指针*/

//定义队列的结构体
typedef struct QueueNode{//队列的结点
    pTreeNode data;
    QueueNode *next;
}QueueNode,*pQueueNode;

typedef struct Queue{
	pQueueNode front;
	pQueueNode rear;
}Queue,*pQueue;
```

//初始化队列
```c
pQueue init(pQueue pq){
	pq->front=(pQueueNode)malloc(sizeof(QueueNode));
	pq->front->next=NULL;
	pq->rear=pq->front;
	return pq;
}
```

#### 二叉树的生成

```c
void create(pTreeNode *t){
	int ch;//当前结点
	scanf_s("%d",&ch);
	if(ch==-1){
		(*t)=NULL;
		return;
	}else{
		(*t)=(pTreeNode)malloc(sizeof(TreeNode));
		(*t)->data=ch;
		printf("请输入%d的左节点数据:",ch);
		create(&((*t)->left));//&((*t)->left) 是取 (*t)->left 的地址，因为 create 函数需要修改当前节点的左子树，所以你需要传递指向左子树的指针的地址。
		printf("请输入%d的右节点数据:",ch);
		create(&((*t)->right));
	}
}
```

#### 入队与出队

```c
//入队
void enqueue(pQueue pq,pTreeNode t){
	pQueueNode pNew=(pQueueNode)malloc(sizeof(QueueNode));
	pNew->data=t;
	pNew->next=NULL;
	pq->rear->next=pNew;//将新创建的队列节点 pNew 添加到队列尾部
	pq->rear=pNew;//最后，这行代码将队列的 rear 指针更新为新添加的节点 pNew
}
//出队
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
```

#### 进行层序遍历

```c
void LevelOrderBinaryTree(pTreeNode t){
	pQueue pq=(pQueue)malloc(sizeof(Queue));
	pq=init(pq);
	enqueue(pq,t);
	while(pq->rear!=pq->front){//若队列不为空
		pTreeNode x=dequeue(pq);//根结点出队
		printf("%d ",x->data);
		if(x->left){//遍历左子树
			enqueue(pq,x->left);
		}
		if(x->right){//遍历右子树
			enqueue(pq,x->right);
		}
	}
}
```

#### 主函数

```c
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

#### 完整代码

```c
#define _CRT_SECURE_NO_WARNINGS
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

## 队列实现

这个简单~浅浅看一下吧~

#### 完整代码

```c
#include <stdio.h>
#include <stdlib.h>

// TreeNode 结构体
typedef struct TreeNode {
    int data;
    struct TreeNode* left;
    struct TreeNode* right;
} TreeNode;

// pTreeNode 定义
typedef TreeNode* pTreeNode;

// 层序遍历函数，使用数组实现
void levelOrderTraversal(pTreeNode root) {
    if (root == NULL) {
        return;
    }

    // 创建一个数组来模拟队列
    pTreeNode queue[100]; // 假定树节点数不超过100
    int front = 0, rear = 0;

    // 将根节点入队
    queue[rear++] = root;

    while (front < rear) {
        pTreeNode current = queue[front++]; // 出队

        // 打印当前节点的值
        printf("%d ", current->data);

        // 将左子节点入队
        if (current->left != NULL) {
            queue[rear++] = current->left;
        }

        // 将右子节点入队
        if (current->right != NULL) {
            queue[rear++] = current->right;
        }
    }
}

int main() {
    // 创建一个二叉树示例
    pTreeNode root = (pTreeNode)malloc(sizeof(TreeNode));
    root->data = 1;
    root->left = (pTreeNode)malloc(sizeof(TreeNode));
    root->left->data = 2;
    root->right = (pTreeNode)malloc(sizeof(TreeNode));
    root->right->data = 3;
    root->left->left = NULL;
    root->left->right = NULL;
    root->right->left = (pTreeNode)malloc(sizeof(TreeNode));
    root->right->left->data = 4;
    root->right->right = NULL;
    root->right->left->left = NULL;
    root->right->left->right = NULL;

    printf("层序遍历如下: ");
    levelOrderTraversal(root);

    // 释放内存
    free(root->right->left);
    free(root->right);
    free(root->left);
    free(root);
    
    return 0;
}

```





