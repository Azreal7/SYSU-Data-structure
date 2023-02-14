# Chapter2.2 栈

定义：限定仅在表尾进行插入和删除操作的线性表。允许插入和删除的一端叫做**栈顶**，另一端叫做**栈底**。

## 应用

### 表达式求值

$表达式\begin{cases}前缀表达式（波兰式）\\中缀表达式\\后缀表达式（逆波兰式）\end{cases}$  例如：$(a+b)*(a-b)\begin{cases}*+ab-ab\\(a+b)*(a-b)\\ab+ab-*\end{cases}$



中缀表达式计算步骤：

1. 操作数栈置空，操作符栈压入算符“#”
2. 依次读入表达式的每个单词
3. 如果是操作数，压入操作数栈
4. 如果是操作符，将操作符与栈顶元素$\theta_1$与读入的操作符$\theta_2$进行优先级比较
   1. 如果栈顶元素优先级低，将$\theta_2$压入操作符栈
   2. 如果相等，弹操作符栈
   3. 如果栈顶元素优先级高，弹出两个操作数，一个运算符，进行计算，并将计算结果压入操作数栈，重复4的判断
5. 直至整个表达式处理完毕



中缀转后缀代码：
```c++
string encode(string& str){
	string nums;
	stack<char> sign;
    bool isnegative=false;
	for(int i=0;i<str.size();i++){
		if(isdigit(str[i])) {
            if(isnegative){
                nums.push_back('-');
                isnegative=false;
            }
            nums.push_back(str[i]);
        }
		else{
            if(str[i]=='-' && (i==0 || str[i-1]=='(' || isoperator(str[i-1]))){
                isnegative=true;
                continue;
            }
			nums.push_back(' ');
			if(sign.empty() || str[i]=='(' || sign.top()=='('){
				sign.push(str[i]);
			}
			else if(str[i]==')'){
                while(sign.top()!='('){
                    nums.push_back(sign.top());
                    sign.pop();
                }
                sign.pop();
            }
            else if(getlevel(str[i])>getlevel(sign.top())){
                sign.push(str[i]);
            }
            else{
                while(!sign.empty() && getlevel(str[i])<=getlevel(sign.top())){
                    nums.push_back(sign.top());
                    sign.pop();
                }
                sign.push(str[i]);
			}
		}
	}
	while(!sign.empty()){
        if(sign.top()!='(' && sign.top()!=')') nums.push_back(sign.top());
		sign.pop();
	}
	return nums;
}
```



### 递归

递归定义的算法分为两个部分：

- 递归基：直接定义最简单情况下的函数值。
- 递归步：通过较为简单情况下的函数值定义一般情况下的函数值。

且递归的算法分为：<font color=red>单路</font>递归、<font color=red>多路</font>递归、<font color=red>间接</font>递归、<font color=red>迭代</font>递归。



对于非递归函数，在函数被递归调用之前，系统先要保存以下两类信息：

1. 调用函数的返回地址
2. 调用函数的局部变量值

当执行完被调用函数，在返回调用函数之前，系统要先**恢复**调用函数的**局部变量值**，然后**返回**调用函数的**返回地址**。

而对于递归函数，系统需要一个**运行时栈**，每一层递归调用所需保存的信息构成运行时栈的一个**工作记录**，每次进入下一层调用时，建立一个新的**工作记录**，并把工作记录进栈成为新的栈顶。



#### 将递归转换为非递归

 在递归的关键部分(判断是否return)标记**断点地址**，之后在原递归调用的地方使用<font color=red>goto语句</font>来进行伪递归，同时将工作记录保留至栈中，用栈顶存储的工作记录来进行操作，当当层递归结束时，栈顶元素出栈。
