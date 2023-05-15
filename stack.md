# 堆栈 Stack

## 1. 堆栈基础

> 堆栈（Stack）：简称为栈。是一种线性表数据结构。只允许在表的一端进行插入和删除。

* **栈顶（Top）**: 栈中允许插入和删除操作的一端。

* **栈底（Bottom）**: 栈中除了允许插入和删除操作的一端之外的另一端。 

* **空栈**: 没有任何数据的空表。

* **入栈**: 即**进栈**，栈的插入操作。

* **出栈**: 即**退栈**，栈的删除操作。

* **LIFO**: stands for "Last In First Out"。栈是一种后进先出的线性表。入栈顺序为`a1, a2, ..., an`, 栈顶元素为`an`。进栈时，最先进入栈的元素在**栈底**，最后进入栈的元素在**栈顶**。每次删除的一定是**栈顶**的元素。

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
