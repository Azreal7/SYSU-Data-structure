# Chapter6 排序

排序的**稳定性**：如果对象序列中有两个对象$r_i$和$r_j$，他们的关键字**相等**，在排序之前$r_i$就在$r_j$之前，若在排序后相对位置仍然不变，则称这个排序方法是稳定的。

## 插入排序

直接插入，O($n^2$)，稳定。

## 希尔排序

希尔排序方法又称为**缩小增量**排序。

<img src="C:\Users\hp\AppData\Local\Temp\WeChat Files\e11780d3c9fa7676eb4425cfa0b83e7.jpg" alt="e11780d3c9fa7676eb4425cfa0b83e7" style="zoom: 67%;" />

## 冒泡排序

设待排序对象序列中的对象个数为n。最多做n-1趟排序。在第i趟中比较r[i-1]与r[i]，如果发生逆序，交换r[i-1]和r[i]。

排序方法**稳定**。

最坏的场景是进行了n-1趟冒泡，时间复杂度为O($n^2$)。

## 快速排序

快速排序先在序列中取定一个**枢轴**，然后整个序列按照与枢轴数字的相对大小，分为左右两个序列。

1. 设一个left=0,right=n-1。

2. right先移动，移动到第一个小于枢轴的数字。

3. left再移动，移动到第一个大于枢轴的数字。

4. 之后交换left和right的值
5. 以此反复，直到left=right，交换left与枢轴的值。
6. 对枢轴两端进行快速排序。

```c++
void quick_sort(int* arr, int left, int right) {
    if(left == right) return;
    int l = left, r = right;
    int pivot = arr[left];
    while(l < r) {
        while(arr[r] > arr[pivot] && l < r) {
            r--;
        }
        while(arr[l] < arr[pivot] && l < r) {
            l++;
        }
        swap(arr[l], arr[r]);
    }
    if(pivot != l) swap(arr[l], arr[pivot]);
    quick_sort(arr, left, piovt - 1);
    quick_sort(arr, pivot + 1, right);
}
```

## 选择排序

两个for每次选取最小值。

## 堆排序

堆排序的关键：

1. 由无序序列建成一个堆。
2. 在输出堆顶元素后，重新调整剩余元素。

### 堆的建立

堆的建立过程分为两个函数**BuildMaxHeap()**和**Heapfy()**。

BuildMaxHeap()的作用就是从数的末端向根部(即数组**从后往前**)调用Heapfy()。

Heapfy()的作用是：

- 比较$k_i$与$k_{i*2+1}$和$k_{i*2+2}$的大小关系。
  - 若$k_i$比两者都小，不进行更换。
  - 若$k_i$只比$k_{2*i+1}$和$k_{2*i+2}$其中一者大，与其**交换**，并顺着那个位置继续调用Heapfy()。
  - 若$k_i$比两者都大，与**较大**的那个进行交换，从那个位置调用Heapfy()。

### 堆的输出

随后每次调整完毕后，输出堆顶元素，设立一个变量x=n-1来划分数组中已输出元素与未输出元素的界限。将堆顶元素与x标记的元素互换，随后将未输出元素重新建立根堆。

## 归并排序

将整个数组划分为多个小数组进行**分治**和**排序**，再将排序好的几个小数组合并。

![d80affaed9e74b6bffd57e239c7738d](C:\Users\hp\AppData\Local\Temp\WeChat Files\d80affaed9e74b6bffd57e239c7738d.jpg)

主要函数如下：

```c++
void MergeSort(int* a, int low, int high) {
    int mid = (low + high) / 2;
    if(low >= high) return;
    MergeSort(a, low, mid);
    MergeSort(a, mid + 1, high);
    Merge(a, low, mid, high); //将两个数组合并为一个有序数组。
}
```

## 基数排序

从各个数字的最低位开始比较。类似桶排序。

![5b12068a8f1985d20d58f48129f4f30](C:\Users\hp\AppData\Local\Temp\WeChat Files\5b12068a8f1985d20d58f48129f4f30.jpg)

## 各个排序复杂度情况对比

![31ed4be12ecf0264e9cac6cecde3388](C:\Users\hp\AppData\Local\Temp\WeChat Files\31ed4be12ecf0264e9cac6cecde3388.jpg)

