---
layout:     post   				    # 使用的布局（不需要改）
title:      Data Structure of Sorting			# 标题 
subtitle:   经典排序  #副标题
date:       2023-12-5 				# 时间
author:     beansugar 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - Data Structure
---

时间过得真快啊，转专业的第一个学期也已过去大半，学到了不少东西。但现在有种眼高手低的感觉，又有种无力感，要学的东西有很多，学校的课程不能落下，又怕基础不牢固  

在这边立个flag吧，寒假学java，wxml，leetcode刷300题，暂且这样~  

以下是正文

今天讨论几种经典排序 :smile: ,主要是书上的代码又臭又长，虽然说，有几个写的还是不错的:laughing:

![Alt text](image.png)

# 冒泡排序

比较相邻的元素。如果第一个比第二个大，就交换他们两个。

对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。这步做完后，最后的元素会是最大的数。

针对所有的元素重复以上的步骤，除了最后一个。

持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。

```c
//冒泡
void bubSort(int arr[], int length) {
	int flag = 1;//优化：若flag不为1；那么说明没有交换，数组有序
	while (length-- && flag) {
		flag = 0;
		for (int i = 0; i <  length; i++) {
			if (arr[i] > arr[i + 1]) {
				flag = 1;
				int tmp = arr[i];
				arr[i] = arr[i + 1];
				arr[i + 1] = tmp;
			}
		}
	}
}
```

# 选择排序

首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置。

再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。

重复第二步，直到所有元素均排序完毕。

```c
//选择
void sortArr(int arr[], int length) {
	for (int i=0;i<length;i++)
	{
		int min = i;
		for (int j = i+1; j < length; j++) {
			if (arr[i] > arr[j]) {
				min = j;
			}
		}
		int tmp = i;
		arr[i] = arr[min];
		arr[min] = arr[tmp];
	}
}
```

# 插入排序

将第一待排序序列第一个元素看做一个有序序列，把第二个元素到最后一个元素当成是未排序序列。

从头到尾依次扫描未排序序列，将扫描到的每个元素插入有序序列的适当位置。（如果待插入的元素与有序序列中的某个元素相等，则将待插入元素插入到相等元素的后面。）

```c  
//插入
void InsertArr(int arr[], int length) {
	for (int i = 1; i < length; i++) {
		for (int j = i; j > 0 && arr[j]<arr[j-1]; j--) {
			int tmp = arr[j];
			arr[j] = arr[j - 1];
			arr[j - 1] = tmp;
		}
	}
}
```

# 希尔排序

选择一个增量序列 t1，t2，……，tk，其中 ti > tj, tk = 1；

按增量序列个数 k，对序列进行 k 趟排序；

每趟排序，根据对应的增量 ti，将待排序列分割成若干长度为 m 的子序列，分别对各子表进行直接插入排序。仅增量因子为 1 时，整个序列作为一个表来处理，表长度即为整个序列的长度。

```c
//希尔（优化版插入）在数据量大的时候速度更快

void shellArr(int arr[], int length) {
	int h = 4;
	int t = length / 3;
	/*while (h < t) {
		h = 3 * h + 1;
	}*/
	while (h>=1)
	{
		for (int i = h; i < length; i++) {
			for (int j = i; j > 0 && arr[j] < arr[j - h]; j-=h) {
				int tmp = arr[j];
				arr[j] = arr[j - h];
				arr[j - h] = tmp;
			}
		}
		h /= 2;//h/=3;
	}
}
```

# 快速排序

- 从数列中挑出一个元素，称为 "基准"（pivot）;

- 重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；

- 递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序；

```c
void quicksort(int arr[],int left, int right)
{
	if (left >= right) {
		return;
	}
	int i = left;
	int j = right;
	int povit = arr[i];
	while (i<j)
	{
		while (i<j && arr[j]>=povit) {
			j--;
		}
		arr[i] = arr[j];
		while (i<j && arr[i]<=povit)
		{
			i++;
		}
		arr[j] = arr[i];
	}
	arr[i] = povit;
	if (left < i - 1) {
		quicksort(arr, left, i - 1);
	}
	if (i + 1 < right) {
		quicksort(arr, i + 1, right);
	}
}

void swap(int arr[], int i, int j) {
	int tmp = arr[i];
	arr[i] = arr[j];
	arr[j] = tmp;
}

void quicksort2(int arr[], int left, int right) {
	if (left >= right) {
		return;
	}
	int pivot = arr[left];
	int i = left + 1;
	int j = left + 1;
	while (j<=right) {
		if (arr[j] < pivot) {
			swap(arr, i, j);
			i++;
		}
		j++;
	}
	swap(arr, left, i - 1);
	quicksort2(arr, left, i - 2);
	quicksort2(arr, i, right);
}

```

# 归并排序

- 申请空间，使其大小为两个已经排序序列之和，该空间用来存放合并后的序列；

- 设定两个指针，最初位置分别为两个已经排序序列的起始位置；

- 比较两个指针所指向的元素，选择相对小的元素放入到合并空间，并移动指针到下一位置；

- 重复步骤 3 直到某一指针达到序列尾；

- 将另一序列剩下的所有元素直接复制到合并序列尾。

```c
//归并排序，假设两个数组都有序
void mergesort(int a[], int alen,int b[], int blen, int* tmp) {
	int i = 0, j = 0, k = 0;
	while (i<alen && j<blen) 
		tmp[k++] = a[i] < b[j] ? a[i++] : b[j++];
		while (i<alen)
			tmp[k++] = a[i++];
		while (j < blen)
			tmp[k++] = b[j++];
}

//归并排序
void mergesort1(int arr[],int l, int r,int * c) {
	if (l == r) return;
	int m = (l + r) / 2;
	mergesort1(arr, l, m,c);
	mergesort1(arr, m + 1, r,c);
	int p1 = l, p2 = m + 1, tot = 0;
	while (p1<=m && p2<=r)
	{
		c[++tot] = arr[p1] <= arr[p2] ? arr[p1++] : arr[p2++];
	}
		while (p1<=m)
		{
			c[++tot] = arr[p1++];
		}
		while (p2 <= r)
		{
			c[++tot] = arr[p2++];
		}
		for (int i = 1; i <= tot; i++) {
			arr[i + l - 1] = c[i];
		}
}
```

# 计数排序

（1）找出待排序的数组中最大和最小的元素
（2）统计数组中每个值为i的元素出现的次数，存入数组C的第i项
（3）对所有的计数累加（从C中的第一个元素开始，每一项和前一项相加）
（4）反向填充目标数组：将每个元素i放在新数组的第C(i)项，每放一个元素就将C(i)减去1

```c
//计数排序
int tmp[100] = { 0 };
void countsort(int arr[], int length) {
	int brr[Maxsize];
	for (int i = 0; i < length; i++) {
		brr[i] = arr[i];
	}

	for (int i = 0; i < length; i++) {
		tmp[arr[i]]++;
	}
	for (int i=0,j = 0; i < 100; i++) {
		for (int k = 0; k < tmp[i];k++) {
			arr[j++] = i;
		}
	}
	//原来的数组的数在现在数组的哪个位置，实现排序的稳定性
	// 排完后在第几位，相同数字，序号小的在前面
	//先做一个前缀和
	int c[1000] = { 0 };
	int r[1000];
	for (int i = 1; i < 100; i++) {
		tmp[i] += tmp[i - 1];
	}
	for (int i = length - 1; i >= 0; --i) {
		r[i] = tmp[brr[i]]--;
	}
	for (int i = 0; i < length; i++) {
		printf("%d ", r[i]);
	}
	printf("\n");
}
```

# 基数排序

明天一定

```c

```

# 完整代码

```c
#include<stdio.h>
#include<stdlib.h>
#include<time.h>


#define Maxsize 10

void initArr(int arr[], int length) {
	for (int i = 0; i < length; i++) {
		arr[i] = rand() % 100;
	}
}

void showArr(int arr[], int length) {
	for (int i = 0; i < length; i++) {
		printf("%d ", arr[i]);
	}
	printf("\n-----------------------\n");
}
//冒泡
void bubSort(int arr[], int length) {
	int flag = 1;//优化：若flag不为1；那么说明没有交换，数组有序
	while (length-- && flag) {
		flag = 0;
		for (int i = 0; i <  length; i++) {
			if (arr[i] > arr[i + 1]) {
				flag = 1;
				int tmp = arr[i];
				arr[i] = arr[i + 1];
				arr[i + 1] = tmp;
			}
		}
	}
}	
//选择
void sortArr(int arr[], int length) {
	for (int i=0;i<length;i++)
	{
		int min = i;
		for (int j = i+1; j < length; j++) {
			if (arr[i] > arr[j]) {
				min = j;
			}
		}
		int tmp = i;
		arr[i] = arr[min];
		arr[min] = arr[tmp];
	}
}
//插入
void InsertArr(int arr[], int length) {
	for (int i = 1; i < length; i++) {
		for (int j = i; j > 0 && arr[j]<arr[j-1]; j--) {
			int tmp = arr[j];
			arr[j] = arr[j - 1];
			arr[j - 1] = tmp;
		}
	}
}
//希尔（优化版插入）在数据量大的时候速度更快

void shellArr(int arr[], int length) {
	int h = 4;
	int t = length / 3;
	/*while (h < t) {
		h = 3 * h + 1;
	}*/
	while (h>=1)
	{
		for (int i = h; i < length; i++) {
			for (int j = i; j > 0 && arr[j] < arr[j - h]; j-=h) {
				int tmp = arr[j];
				arr[j] = arr[j - h];
				arr[j - h] = tmp;
			}
		}
		h /= 2;//h/=3;
	}
}

void quicksort(int arr[],int left, int right)
{
	if (left >= right) {
		return;
	}
	int i = left;
	int j = right;
	int povit = arr[i];
	while (i<j)
	{
		while (i<j && arr[j]>=povit) {
			j--;
		}
		arr[i] = arr[j];
		while (i<j && arr[i]<=povit)
		{
			i++;
		}
		arr[j] = arr[i];
	}
	arr[i] = povit;
	if (left < i - 1) {
		quicksort(arr, left, i - 1);
	}
	if (i + 1 < right) {
		quicksort(arr, i + 1, right);
	}
}

void swap(int arr[], int i, int j) {
	int tmp = arr[i];
	arr[i] = arr[j];
	arr[j] = tmp;
}

void quicksort2(int arr[], int left, int right) {
	if (left >= right) {
		return;
	}
	int pivot = arr[left];
	int i = left + 1;
	int j = left + 1;
	while (j<=right) {
		if (arr[j] < pivot) {
			swap(arr, i, j);
			i++;
		}
		j++;
	}
	swap(arr, left, i - 1);
	quicksort2(arr, left, i - 2);
	quicksort2(arr, i, right);
}

//归并排序，假设两个数组都有序
void mergesort(int a[], int alen,int b[], int blen, int* tmp) {
	int i = 0, j = 0, k = 0;
	while (i<alen && j<blen) 
		tmp[k++] = a[i] < b[j] ? a[i++] : b[j++];
		while (i<alen)
			tmp[k++] = a[i++];
		while (j < blen)
			tmp[k++] = b[j++];
}

//归并排序
void mergesort1(int arr[],int l, int r,int * c) {
	if (l == r) return;
	int m = (l + r) / 2;
	mergesort1(arr, l, m,c);
	mergesort1(arr, m + 1, r,c);
	int p1 = l, p2 = m + 1, tot = 0;
	while (p1<=m && p2<=r)
	{
		c[++tot] = arr[p1] <= arr[p2] ? arr[p1++] : arr[p2++];
	}
		while (p1<=m)
		{
			c[++tot] = arr[p1++];
		}
		while (p2 <= r)
		{
			c[++tot] = arr[p2++];
		}
		for (int i = 1; i <= tot; i++) {
			arr[i + l - 1] = c[i];
		}
}


//计数排序
int tmp[100] = { 0 };
void countsort(int arr[], int length) {
	int brr[Maxsize];
	for (int i = 0; i < length; i++) {
		brr[i] = arr[i];
	}

	for (int i = 0; i < length; i++) {
		tmp[arr[i]]++;
	}
	for (int i=0,j = 0; i < 100; i++) {
		for (int k = 0; k < tmp[i];k++) {
			arr[j++] = i;
		}
	}
	//原来的数组的数在现在数组的哪个位置，实现排序的稳定性
	// 排完后在第几位，相同数字，序号小的在前面
	//先做一个前缀和
	int c[1000] = { 0 };
	int r[1000];
	for (int i = 1; i < 100; i++) {
		tmp[i] += tmp[i - 1];
	}
	for (int i = length - 1; i >= 0; --i) {
		r[i] = tmp[brr[i]]--;
	}
	for (int i = 0; i < length; i++) {
		printf("%d ", r[i]);
	}
	printf("\n");
}



int main() 
{	
	srand((unsigned int)time(NULL));
	int arr[Maxsize];
	int c[2000];
	/*initArr(arr, Maxsize);
	showArr(arr, Maxsize);
	bubSort(arr, Maxsize);
	showArr(arr, Maxsize);
	sortArr(arr, Maxsize);
	showArr(arr, Maxsize);
	InsertArr(arr, Maxsize);
	showArr(arr, Maxsize);
	shellArr(arr, Maxsize);
	quicksort2(arr, 0, Maxsize - 1);
	showArr(arr, Maxsize);*/
	initArr(arr, Maxsize);
	showArr(arr, Maxsize);
	/*mergesort1(arr, 0, Maxsize - 1, c);*/
	countsort(arr, 10);
	showArr(arr, Maxsize);

	//归并排序使用该数组
	/*int a[5] = { 1,3,5,7,9 };
	int b[5] = { 0,2,4,6,8 };
	int tmp[10];
	mergesort(a, 5, b, 5, tmp);
	showArr(tmp, 10);*/
	return 0;
}

```