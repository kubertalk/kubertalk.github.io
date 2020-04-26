---
title: 八大排序算法  by C++ (转)
date: 2017-04-28 15:20:55
categories: 
	- 数据结构
tags: 
	- 数据结构
	- 冒泡排序
	- 插入排序
	- 选择排序
	- 归并排序
	- 快速排序
	- 希尔排序
	- 堆排序
	- 桶排序
	- 基数排序	
thumbnailImage: mushroom.jpg
thumbnailImagePosition: left
autoThumbnailImage: true
coverImage: galaxy.jpg
coverCaption: "A beautiful galaxy"
---

八大排序算法  by C++ (转)
<!--more-->
<!--toc-->

#### 八大排序算法  by C++ (转)

##### 冒泡排序

```cpp
#include <iostream>
using namespace std;

void BubbleSort(int arr[], int len)
{
​	for (int i=0;i<len-1;i++)
​	{
​		for (int j=0;j<len-i-1;j++)
​		{
​			if (arr[j] > arr[j+1])
​			{
​				int tmp = arr[j];
​				arr[j] = arr[j+1];
​				arr[j+1] = tmp;
​			}
​		}
​	}
}
```



##### 插入排序

```c++
#include <iostream>
using namespace std;

void InsertSort(int arr[], int len)
{
​	int tmp;
​	for (int i=1;i<len;i++)
​	{
​		for (int j=i-1;j>=0;j--)
​		{
​			if (arr[i] < arr[j])
​			{
​				int tmp = arr[j];
​				arr[j] = arr[i];
​				arr[i] = tmp;
​				i=j;
​			}
​			else
​				break;
​		}
​	}
}
```



##### 选择排序

```
#include <iostream>
using namespace std;

void SelectSort(int arr[], int len)
{
​	int i,j,min, tmp;
​	for (i=0;i<len-1;i++)
​	{
​		min = i;
​		for (j=i+1;j<len;j++)
​		{
​			if (arr[min] > arr[j])
​			{
​				min = j;
​			}
​		}
​		tmp = arr[min];
​		arr[min] = arr[i];
​		arr[i] = tmp;
​	}
}
```



##### 归并排序

```c++
#include <iostream>
using namespace std;

void MergeArray(int arr[], int l, int r, int mid)
{
​	int* temp = (int*)malloc((r-l+1)*sizeof(int));
​	int i=l, j = mid+1, k=0;
​	while(i<=mid && j<=r)
​	{
​		if (arr[i] <= arr[j])
​		{
​			temp[k++] = arr[i];
​			i++;
​		}
​		else
​		{
​			temp[k++] = arr[j];
​			j++;
​		}
​	}
​	while(i<=mid)
​	{
​		temp[k++] = arr[i];
​		i++;
​	}
​	while(j<=r)
​	{
​		temp[k++] = arr[j];
​		j++;
​	}
​	for (i=0;i<k;i++)
​	{
​		arr[l+i] = temp[i];
​	}
​	free(temp);
}

void MergeSort(int arr[], int l, int r)
{
​	if (l < r)
​	{
​		int mid = (l + r)/2;
​		MergeSort(arr, l, mid);
​		MergeSort(arr, mid+1, r);
​		MergeArray(arr, l, r, mid);
​	}
​	
​	
}
```



##### 快速排序

```c++
#include <iostream>
using namespace std;

int AdjustArr(int arr[], int l, int r)
{
​	int i=l,j=r,tmp=arr[l];
​	while(i<j)
​	{
​		while(arr[j] > tmp && i<j)
​		{
​			j--;
​		}
​		if (i<j)
​		{
​			arr[i++] = arr[j];// ++
​		}
​		while(arr[i] < tmp && i<j)
​		{
​			i++;
​		}
​		if (i<j)
​		{
​			arr[j--] = arr[i];
​		}
​	}
​	arr[i] = tmp;
​	return i;
}
void QuickSort(int arr[], int l, int r)
{
​	if (l < r)
​	{
​		int k = AdjustArr(arr, l, r);
​		QuickSort(arr,l,k-1);
​		QuickSort(arr,k+1,r);
​	}
}
```



##### 希尔排序

```c++
#include <iostream>
using namespace std;

void ShellSort(int arr[], int len)
{
​	int step = len / 2;
​	int tmp;
​	for (;step > 0;step /= 2)
​	{
​		for (int i=0;i<step;i++)
​		{
​			for (int j=i+step;j<len;j+=step)
​			{
​				for (int k=j-step;k>=0;k-=step)
​				{
​					if (arr[j] < arr[k])
​					{
​						tmp = arr[j];
​						arr[j] = arr[k];
​						arr[k] = tmp;
​						j = k;// attention

​					}
​					else
​						break;
​				}
​			}
​		}
​	}
}
```



##### 堆排序

```c++
#include <iostream>
using namespace std;

void HeapDown(int arr[], int first, int last)
{
​	int i=first, j=2*i+1,tmp;
​	while(j<last)
​	{
​		if (arr[j] < arr[j+1])
​		{
​			j++;
​		}
​		if (arr[i] < arr[j])
​		{
​			tmp = arr[j];
​			arr[j] = arr[i];
​			arr[i] = tmp;
​			i = j;
​			j = 2*i+1;
​		}
​		else
​			break;
​	}
}

void HeapSort(int arr[], int len)
{
​	int tmp;
​	for (int i = len/2-1; i>=0; i--)
​	{
​		HeapDown(arr, i, len-1);
​	}

​	for (int i=len-1;i>0;i--)
​	{
​		tmp = arr[0];
​		arr[0] = arr[i];
​		arr[i] = tmp;
​		HeapDown(arr, 0, i-1);
​	}
}
```



##### 桶排序

```c++
#include <iostream>
using namespace std;

void BucketSort(int arr[], int len, int nExp)
{
​	int bucketArr[10] = {0};
​	int *outputs = (int*)malloc(len*sizeof(int));
​	for(int i=0;i<len;i++)
​	{
​		bucketArr[(arr[i]/nExp)%10]++;
​	}
​	for(int j=1;j<len;j++)
​	{
​		bucketArr[j] += bucketArr[j-1];
​	}
​	for (int i=len-1;i>=0;i--)// attention
​	{
​		outputs[bucketArr[(arr[i]/nExp)%10]-1] = arr[i];
​		bucketArr[(arr[i]/nExp)%10]--;
​	}
​	for (int i=0;i<len;i++)
​	{
​		arr[i] = outputs[i];
​	}
​	free(outputs);
}
```



##### 基数排序

```c++
#include <iostream>
using namespace std;

void RadixSort(int arr[], int len)
{
​	int i, nMax = arr[0], nExp;
​	for (i=1;i<len;i++)
​	{
​		if (nMax < arr[i])
​		{
​			nMax = arr[i];
​		}
​	}

​	for (nExp = 1; nMax/nExp>0; nExp *= 10)
​	{
​		BucketSort(arr, len, nExp);
​	}

}
```



#### Just Test It

```c++
#include <iostream>
using namespace std;

void main()
{
​	int arr[] = {49,38,65,97,76,13,27,49,55,0};
​	int len = 10;
​	//BubbleSort(arr, len);
​	//InsertSort(arr, len);
​	//SelectSort(arr, len);
​	//MergeSort(arr, 0, len-1);
​	//QuickSort(arr, 0, len-1);
​	//ShellSort(arr, len);
​	//HeapSort(arr, len);
​	RadixSort(arr, len);
​	for (int i=0;i<len;i++)
​	{
​		cout<<arr[i]<<" ";
​	}
​	cout<<endl;
​	system("Pause");
}
```



