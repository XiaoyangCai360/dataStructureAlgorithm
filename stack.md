# 堆栈 Stack

## 1. 堆栈基础

> 堆栈（Stack）：简称为栈。是一种线性表数据结构。只允许在表的一端进行插入和删除。

* **栈顶（Top）**: 栈中允许插入和删除操作的一端。

* **栈底（Bottom）**: 栈中除了允许插入和删除操作的一端之外的另一端。 

* **空栈**: 没有任何数据的空表。

* **入栈**: 即**进栈**，栈的插入操作。

* **出栈**: 即**退栈**，栈的删除操作。

* **LIFO**: stands for "Last In First Out"。栈是一种**后进先出**的线性表。入栈顺序为`a1, a2, ..., an`, 栈顶元素为`an`。进栈时，最先进入栈的元素在**栈底**，最后进入栈的元素在**栈顶**。每次删除的一定是**栈顶**的元素。

## 2. 堆栈类型和基本操作
堆栈可以分为**顺序栈**和**链式栈**。

* **顺序栈**：堆栈的顺序存储结构。利用一组**连续内存**依次存放自栈顶到栈底的元素，使用指针`top`指向栈顶。
  
* **链式栈**：堆栈的链式存储结构，利用单链表的方式实现栈。栈中元素按照插入顺序依次插入到第一个节点**之前**。指针`top`指向栈顶元素，所以`top`永远指向**头节点**`head`。

### 2.1 堆栈基本操作
堆栈理论上具备线性表所有的操作，但由于**后进先出LIFO**特性，堆栈操作做出了特定改变。

堆栈基本操作如下：

* **空栈初始化**：创立一个空栈，定义栈的大小`size`和栈顶指针`size`。

* **判断栈是否为空**：堆栈为空时返回`True,`否则返回`False`。一般用于顺序栈中删除元素和获取当前栈顶元素。

* **判断栈是否已满**：堆栈已满时返回`True,`否则返回`False`。一般用于顺序栈中插入元素和获取当前栈顶元素。

* **插入元素**：即进栈，入栈。相当于在线性表最后元素后面加入一个元素，并改变栈顶指针`top`的位置。

* **删除元素**：即退栈，出栈。相当于在线性表最后元素后面删除一个元素，并改变栈顶指针`top`的位置。

* **获取栈顶元素**：相当于获取线性表最后元素，**不**改变栈顶指针`top`的位置。

### 2.2 堆栈顺序存储实现

* 借助于**数组**来实现堆栈的顺序存储。

* 在`Python`中使用`list`来实现顺序栈。

* 顺序栈保留了线性存储内存空间的缺点，即在栈存满或者需要重新调配存储空间时需要大量移动元素。

具体实现：

* **空栈初始化**：使用列表`list`创建一个空栈，定义栈大小`self.size`，令栈顶指针指向`-1`。

```
class Stack:
    # initilize an empty stack
    def __init__(self, size==100):
        self.stack = [] # 空list创建空栈
        self.size = size # stack大小有限
        self.top = -1
```

* **判断栈是否为空**：当栈顶指针`self.top`等于`-1`时说明栈为空，返回`True,`否则返回`False`。

```
    # check if the stack is empty
    def isEmpty(self):
        return self.top == -1
```

* **判断栈是否已满**：当栈顶指针`self.top`等于`self.size - 1`时说明堆栈已满，返回`True,`否则返回`False`。 

```
    # check if the stack is full
    def isFull(self):
        return self.top == self.size - 1
```

* **插入元素**：即`Push`。先判断堆栈是否**已满**，如果已满抛出异常。如果没满，在`self.stack`末尾插入元素，`self.top`向右移动一位。

```
    # add one element to the stack
    def push(self, val):
        if self.isFull():
            raise Exception('Stack is full')
        else:
            self.stack.append(val)
            self.top += 1
```

* **删除元素**：即`Pop`。先判断堆栈是否**为空**，如果为空抛出异常。如果不为空，在`self.stack`末尾删除元素，`self.top`向左移动一位。

```
    # delete one element from the stack
    def pop(self):
        if self.isEmpty():
            raise Exception('Stack is empty')
        else:
            self.stack.pop()
            self.top -= 1
```

* **获取栈顶元素**：即`Peek`。先判断堆栈是否**为空**，如果为空抛出异常。如果不为空，返回`self.top`指向的栈顶元素，即`self.stack[self.top]`。

```
    # get the top element of the stack
    def peek(self):
        if self.isEmpty():
            raise Exception('Stack is empty')
        else:
            return self.stack[self.top]
```

### 2.3 堆栈链式存储实现

* 借助于**链表**来实现**链式栈**。

* 在`Python`中构建链式节点`Node`。

* 约定栈顶指针`self.top`指向栈顶元素所在的位置。

具体实现：

* **链式节点**：构造链式节点`Node`类。

```
# helper Node class
class Node:
    def __init__(self, val):
        self.val = val
        self.next = None
```

* **空栈初始化**：令栈顶指针`self.top`指向`None`。

```
class Stack:
    def __init__(self, top):
        self.top = None
```

* **判断栈是否为空**：当栈顶指针`self.top`等于`None`时说明栈为空，返回`True,`否则返回`False`。

```
    # check if the stack is empty
    def isEmpty(self):
        return self.top == None
```

* **插入元素**：即`Push`。创建值为`val`的节点，插入到链表头节点之前，并使栈顶指针`self.top`指向新的头节点。

```
    # add one element to the stack
    def push(self, val):
        cur = Node(val)
        cur.next = self.top
        self.top = cur
```

* **删除元素**：即`Pop`。先判断堆栈是否**为空**，如果为空抛出异常。如果不为空，先定义辅助节点`cur`指向栈顶指针`self.top`指向的当前头节点，然后让栈顶指针向**右**移动一位，删除`cur`指向的节点。

```
    # delete one element from the stack
    def pop(self):
        if self.isEmpty():
            raise Exception('Stack is empty')
        else:
            cur = self.top
            self.top = self.top.next
            del cur
```

* **获取栈顶元素**：即`Peek`。先判断堆栈是否**为空**，如果为空抛出异常。如果不为空，返回`self.top`指向的栈顶节点的`val`，即`self.top.val`。

```
    # get the top element of the stack
    def peek(self):
        if self.isEmpty():
            raise Exception('Stack is empty')
        else:
            return self.top.val
```

## 3. 堆栈的应用

堆栈是最常用的**辅助**数据结构。其应用通常分为两个方面：
1. 使用堆栈作为辅助数据结构，临时存储信息，以便后面操作中用到
   例如： 操作系统中的函数调用栈，浏览器的前进后退等

2. 利用**后进先出LIFO**原则，保障特定存储顺序
   例如： 翻转一组元素顺序，火车调度等

### 3.1 堆栈例题：[20 有效的括号 Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

#### 3.1.1 题目描述

描述：给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串`s`。

要求：判断字符串`s`是否有效（即括号是否匹配）。

说明：有效字符串需满足：
* 左括号必须用相同类型的右括号闭合。
* 左括号必须以正确的顺序闭合。

#### 3.1.2 解题思路

1. 先判断字符串长度，如果括号匹配，字符串长度应为偶数，如果字符串长度为奇数，返回`False`。

2. 使用堆栈保存为匹配的**左**括号，一次遍历整个字符串`s`
   1. 如果遍历到**左**括号，将其入栈
   2. 如果遍历到**右**括号，先看栈顶指针指向的左括号与当前右括号是否匹配
      1. 如果匹配，则让栈顶指针指向的左括号出栈
      2. 如果不匹配，说明字符串`s`中的括号不匹配，返回`False`

3. 遍历完，判断栈是否为空
   1. 如果栈为空，说明字符串`s`中的括号匹配，返回`True`
   2. 如果栈为空，说明字符串`s`中的括号不匹配，返回`False`

具体代码实现：

```
class Solution(object):
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """

        # 判断字符串长度是否是奇数，如果是，返回false
        if len(s) % 2 == 1:
            return False

        # 实现顺序栈
        stack = list()
        
        # 遍历数组，如果遍历左括号，入栈
        # 如果遍历到右括号，判断是否与栈顶左括号匹配
        # 如果匹配，栈顶左括号出栈
        # 如果不匹配，返回false

        for char in s:
            if char == '(' or char == '[' or char == '{':
                stack.append(char)
            elif char == ')':
                if len(stack) != 0 and stack[-1] == '(':
                    stack.pop()
                else:
                    return False
            elif char == ']':
                if len(stack) != 0 and stack[-1] == '[':
                    stack.pop()
                else:
                    return False
            else:
                if len(stack) != 0 and stack[-1] == '{':
                    stack.pop()
                else:
                    return False
        # 遍历完判断栈是否为空，如果为空返回true，
        # 如果不为空说明有单独的左括号，返回false
        if len(stack) == 0:
            return True
        return False
```
* 时间复杂度:`O(n)`。
* 空间复杂度:`O(1)`

### 3.2 堆栈例题：[227 基本计算器II Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)

#### 3.2.1 题目描述
描述：给定一个字符串表达式`s`，表达式中所有整数为非负整数，运算符只有 `+`、`-`、`*`、`/`，没有括号。

要求：实现一个基本计算器来计算并返回它的值。

#### 3.2.2 解题思路
乘除优先加减，可以使用栈来保存所有乘除后的整数值，然后再求整个栈的和
具体做法：
1. 遍历字符串`s`，先遍历数字直到遇到第一个符号，把完整数字记为`num`，`num`入栈
2. 用`op`表示遇到的符号，默认为`+`
3. 如果遇到数字，继续往后遍历得到完整数字，用`num`表示下一个`op`之前的完整数字，并判断当前`op`的符号
   1. 如果`op == +`，`num`直接入栈
   2. 如果`op == -`，`-num`入栈
   3. 如果`op == *`，`num`与当前栈顶指针指向的`top`相乘，即`num *= top`，然后入栈
   4. 如果`op == /`，`num`与当前栈顶指针指向的`top`相除，即`num /= top`，然后入栈
4. 最后计算整个栈的和，并返回

具体代码实现：

```
class Solution(object):
    def calculate(self, s):
        """
        :type s: str
        :rtype: int
        """

        # 顺序栈
        stack, num, op = [], 0, '+'
        s += '+' # 让最后一个num进栈

        for char in s:
            if char == '': # 空字符继续
                i += 1
                continue
            if char.isdigit(): # num
                num = 10*num + int(char)
            elif char in "+-*/":
                if op == '+': 
                    # 如果op == +, num入栈
                    stack.append(num)
                elif op == '-':
                    # 如果op == -, -num入栈
                    stack.append(-num)
                elif op == '*':
                    # 如果op == *, num*top入栈, top出栈
                    top = stack.pop()
                    stack.append(num * top)
                elif op == '/':
                    # 如果op == /, num/top入栈, top出栈
                    top = stack.pop()
                    stack.append(int(float(top) / num)) # float(top) 因为 round toward zero
                num, op = 0, char # 更新 num = 0， op = 新的操作
        return sum(stack)
```

* 时间复杂度:`O(n)`。因为字符串`s`中每个元素都只遍历一边。
* 空间复杂度:`O(n)`

## 4. 堆栈基础相关题目
| 题号 | 标题 | 标签 | 难度 |
| ----------- | ----------- | ----------- | ----------- |
| 0020| [有效的括号 Valid Parentheses](https://leetcode.com/problems/valid-parentheses/) | 堆栈 | 简单
| 1047| [删除字符串中的所有相邻重复项 Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/description/)| 堆栈 | 简单
| 0227| [基本计算器II Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/) | 堆栈 | 中等
