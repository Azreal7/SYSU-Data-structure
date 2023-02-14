# Chapter2.3 队列

定义：操作受限的线性表，只允许在一端进行元素的插入，另一端进行元素的删除。

## 队列连续设计存储表示和实现

利用一组连续的存储单元（<font color=red>一维数组</font>）依次存放从队首到队尾的各个元素，成为<font color=red>顺序队列</font>。

此时的ADT表示：

```
#defint Max_queue_size 100
typedef struct queue {
	ElemType Queue_array[Max_queue_size];
	int front;
	int rear;
}SqQueue;
```

**队头指针总是指向队头元素的前一个位置**。

|          |                |
| -------- | -------------- |
| 存储空间 | 0...m-1        |
| 初始     | front=0,rear=0 |
| 入队     | rear=rear+1    |
| 出队     | front=front+1  |
| 队空     | rear=front     |
| 队满     | rear-front=m   |

但是连续存储的普通队列会存在**队列溢出**现象。（rear已经指向了最后位置，不能再加入元素，但是数组仍有空余，<font color=red>假溢出</font>。

因此这里引出了**循环队列**。

## 循环队列

为了克服上述**假溢出**现象的方法是：将为队列分配的<font color=blue>向量空间看成一个首尾相接的圆环</font>，并称这种队列为**循环队列**。

<img src="C:\Users\hp\AppData\Roaming\Typora\typora-user-images\image-20230119224838005.png" alt="image-20230119224838005" style="zoom: 33%;" />



|      |                      |
| ---- | -------------------- |
| 入队 | rear=(rear+1)%m      |
| 出队 | front=(front+1)%m    |
| 队空 | **rear=front**       |
| 队满 | **(rear+1)%m=front** |

## 队列的链式表示和实现

ADT表示：

```
数据元素结点定义
typedef struct Qnode
{
	ElemType data;
	struct Qnode *next;
}QNode;

指针结点定义：
typedef struct link_queue
{
	QNode *front, *rear;
}Link_Queue
```

## 队列应用：划分子集问题

### 问题描述

已知集合$A={a_1,a_2,...a_n}$，以及集合上的关系$R=\{(a_i,a_j)|a_i,a_j\in A,i\not=j\}$,其中$(a_i,a_j)$表示i和j存在冲突关系，要求将A划分为**互不相交**的子集$A_1,A_2,...A_k(k\leq n)$，使得子集中元素均无冲突关系，且要求划分的子集个数**尽量少**。

### 算法基本思想

利用**循环**筛选。从第一个元素开始，凡与第一个元素无冲突的元素化为一组，再将剩下元素重新找出互不冲突的归第二组，直到所有元素进组。

### 所用数据结构

- <font color=red>冲突关系矩阵</font>

  - R\[i][j] = 1，i,j有冲突

  - R\[i][j] = 0，i,j无冲突

- <font color=red>循环队列cq[n]</font>，用来存放将要参与划分的元素
- 数组result[n]存放每个元素的分组号
- 工作数组newr[n]，存放当前分组情况下互不冲突的元素。

### 工作过程

1. 将A中元素都放入cq之中，标记此时组号group=1。
2. 第一个元素出队，将R中对应的行拷贝到newr中，下一个元素出队。
   1. 如果下一个出队的元素在newr中对应位置为1，有冲突，将其重新插入cq队尾，参加下一次分组。
   2. 如果对应位置为0，无冲突，则划归本组，且将其R中对应行中的1放入newr之中。
3. 如此反复，直到所有元素依次出队。