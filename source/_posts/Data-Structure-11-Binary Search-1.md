---
title: 数据结构篇笔记-11-二分查找(上)
date: 2018-10-31 09:40:55
categories: 
	- 数据结构
tags: 
	- 数据结构
	- 二分查找
thumbnailImage: mushroom.jpg
thumbnailImagePosition: left
autoThumbnailImage: true
coverImage: galaxy.jpg
coverCaption: "A beautiful galaxy"

---

复习数据结构笔记之二分查找(上)
<!--more-->
<!--toc-->

### 数据结构篇-11-二分查找(上)
#### 一、什么是二分查找？
二分查找针对的是一个有序的数据集合，每次通过跟区间中间的元素对比，将待查找的区间缩小为之前的一半，直到找到要查找的元素，或者区间缩小为0。
#### 二、时间复杂度分析？
##### 1.时间复杂度

​	假设数据大小是n，每次查找后数据都会缩小为原来的一半，最坏的情况下，直到查找区间被缩小为空，才停止。所以，每次查找的数据大小是：n，n/2，n/4，…，n/(2^k)，…，这是一个等比数列。当n/(2^k)=1时，k的值就是总共缩小的次数，也是查找的总次数。而每次缩小操作只涉及两个数据的大小比较，所以，经过k次区间缩小操作，时间复杂度就是O(k)。通过n/(2^k)=1，可求得k=log2n，所以时间复杂度是O(logn)。

##### 2.认识O(logn)

1. 这是一种极其高效的时间复杂度，有时甚至比O(1)的算法还要高效。为什么？
2. 因为logn是一个非常“恐怖“的数量级，即便n非常大，对应的logn也很小。比如n等于2的32次方，也就是42亿，而logn才32。
3. 由此可见，O(logn)有时就是比O(1000)，O(10000)快很多。

#### 三、如何实现二分查找？

##### 1.循环实现

代码实现：

```java
public int binarySearch1(int[] a, int val){
int start = 0;
int end = a.length - 1;
while(start <= end){
int mid = start + (end - start) / 2;
if(a[mid] > val) end = mid - 1;
else if(a[mid] < val) start = mid + 1;
else return mid;
}
return -1;
}
```

注意事项：

1. 循环退出条件是：start<=end，而不是start<end。
2. mid的取值，使用mid=start + (end - start) / 2，而不用mid=(start + end)/2，因为如果start和end比较大的话，求和可能会发生int类型的值超出最大范围。为了把性能优化到极致，可以将除以2转换成位运算，即start + ((end - start) >> 1)，因为相比除法运算来说，计算机处理位运算要快得多。
3. start和end的更新：start = mid - 1，end = mid + 1，若直接写成start = mid，end=mid，就可能会发生死循环。

##### 2.递归实现

```
public int binarySearch(int[] a, int val){
return bSear(a, val, 0, a.length-1);
}
private int bSear(int[] a, int val, int start, int end) {
if(start > end) return -1;
int mid = start + (end - start) / 2;
if(a[mid] == val) return mid;
else if(a[mid] > val) end = mid - 1;
else start = mid + 1;
return bSear(a, val, start, end);
}
```

### 四、使用条件（应用场景的局限性）

1. 二分查找依赖的是顺序表结构，即数组。
2. 二分查找针对的是有序数据，因此只能用在插入、删除操作不频繁，一次排序多次查找的场景中。
3. 数据量太小不适合二分查找，与直接遍历相比效率提升不明显。但有一个例外，就是数据之间的比较操作非常费时，比如数组中存储的都是长度超过300的字符串，那这是还是尽量减少比较操作使用二分查找吧。
4. 数据量太大也不是适合用二分查找，因为数组需要连续的空间，若数据量太大，往往找不到存储如此大规模数据的连续内存空间。

### 五、思考

如何在1000万个整数中快速查找某个整数？

1. 1000万个整数占用存储空间为40MB，占用空间不大，所以可以全部加载到内存中进行处理;
2. 用一个1000万个元素的数组存储，然后使用快排进行升序排序，时间复杂度为O(nlogn);
3. 在有序数组中使用二分查找算法进行查找，时间复杂度为O(logn)
   2.如何编程实现“求一个数的平方根”？要求精确到小数点后6位？
4. LeetCode二分查找相关练习：https://leetcode-cn.com/tag/binary-search/