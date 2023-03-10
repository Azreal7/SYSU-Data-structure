# Chapter1 前置知识

## ADT

定义：ADT的形式化定义是三元组：$ADT=\{D, S, P\}$，其中：D是<font color=orange>数据对象</font>，S是D上的<font color=orange>关系集</font>，P是对D的<font color=orange>基本操作集</font>。其仅仅是一组逻辑特性描述，与其在计算机内的表示与实现无关。

抽象数据类型是为了**求解问题**而定义的数据类型，而数据类型可以看作是已经**实现**了的抽象数据类型。

## 算法

算法的五个重要特性：<font color=red>确定性</font>、<font color=red>能行性</font>、<font color=red>输入</font>、<font color=red>输出</font>、<font color=red>有穷性/有限性</font>

### 算法的描述方法

- 自然语言：容易理解，但是**冗长**且有**二义性**。
- 流程图：**处理流程直观**，但是缺少**严密性**、**灵活性**。
- 伪代码：**表达能力强**、**抽象性强**、**容易理解**。
- 程序设计语言：**能被计算机直接执行**，但是**抽象性差**，对语言要求高，且**正误难以判断**。

### 算法的评价标准

1. 正确性
2. 可读性
3. 健壮性：容错处理
4. 通用性：对一般的数据集合都成立
5. 效率与存储量需求
