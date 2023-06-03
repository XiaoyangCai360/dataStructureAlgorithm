# 队列 Queue

## 1. 队列基础

> 队列（Queue）：一种线性表数据结构，只允许在表的一端进行**插入**操作，在表的另一端进行**删除**操作。

* **队尾（Rear）**: 队列中允许**插入**操作的一端。

* **队头（Front）**: 队列中允许**删除**操作的一端。

* **空队**: 没有任何数据的空队列。

* **入队**: 队列的插入操作。

* **出队**: 队列的删除操作。

* **FIFO**: stands for "First In First Out"。队列是一种**先进先出**的线性表。入队顺序为`a1, a2, ..., an`, 队头元素为`a1`，队尾元素为`an`。入队时，最先入队的元素在**队头**，最后入队的元素在**队尾**。每次删除的一定是**队头**的元素，即最**先**进入队列的元素。

## 2. 队列类型和基本操作
堆栈可以分为**顺序存储的队列**和**链式存储的队列**。

* **顺序存储的队列**：队列的顺序存储结构。利用一组**连续内存**依次存放自队头到队尾的元素，同时使用指针`front`指向队头元素，使用指针`rear`指向队尾元素。

* **链式存储的队列**：队列的链式存储结构。利用**单链表**实现队列。元素按照插入顺序依次插入到第一个节点之后，同时使用指针`front`指向**头节点**，也就是队头元素，使用指针`rear`指向队尾元素。

**注意**：`front`和`rear`的位置不固定，有时候会让`front`指向队头元素的前一个位置，`rear`指向队尾元素的下一个元素。

### 2.1 队列的基本操作

* **初始化空队列**：创建一个大小为`size`的空队列，定义队头元素指针`front`，队尾元素指针为`rear`。

* **判断队列是否为空**：队列为空时返回`True,`否则返回`False`。一般用于**出队操作**和**获取队头元素**。

* **判断队列是否为满**：队列为满时返回`True,`否则返回`False`。一般用于顺序队列**插入操作**。

* **插入元素**：即**入队**。相当于在线性表最后元素后面加入一个元素，并改变队尾指针`rear`的位置。

* **删除元素**：即**出队**。相当于在线性表删除第一个元素，并改变队头指针`front`的位置。

* **获取队头元素**：相当于获取线性表**第一个**元素，**不**改变队头指针`front`和队尾指针`rear`的位置。

* **获取队尾元素**：相当于获取线性表**最后**一个元素，**不**改变队头指针`front`和队尾指针`rear`的位置。

### 2.2 队列顺序存储实现
* 借助于**数组**来实现队列的顺序存储。

* 在`Python`中使用`list`来实现顺序队列。

* **注意**： 约定队头指针`self.front`指向队头元素的**前一个**位置，队尾指针`self.rear`指向**队尾元素**所在位置。
  
具体实现：

* **初始化空队列**：创建一个空队列`self.queue`，定义大小为`self.size`，定义队头元素指针`self.front`和队尾元素指针为`self.rear`都指向`-1`。

```
class queue:
    # initilize an empty queue
    def __init__(self, size==100):
        self.queue = [None for _ in range(size)]
        self.size = size    # 空队列大小有限
        self.front = -1
        self.rear = -1
```

* **判断队列是否为空**：如果队头元素指针`self.front`和队尾元素指针`self.rear`相等，则说明队列为空，否则不为空。

``` 
    # check if the queue is empty
    def isEmpty(self):
        return self.front == self.rear
```

* **判断队列是否为满**：如果队尾元素指针`self.rear`指向最后一个位置，即`self.rear = self.size - 1`，说明队列已满，否则不满。

```
    # check if the queue is full
    def isFull(self):
        return self.rear == self.size - 1
```

* **插入元素**：即`Enqueue`。先判断队列是否**已满**，如果已满抛出异常。如果没满，`self.rear`向右移动一位，再进行赋值操作，此时`self.rear`指向队尾元素。
```
    # add one element to the queue
    def enqueue(self, val):
        if self.isFull():
            raise Exception("Queue is full")
        else:
            self.rear += 1
            self.queue[self.rear] = val
```

* **删除元素**：即`Dequeue`。先判断队列是否**为空**，如果为空抛出异常。如果不为空，`self.front`指向位置赋值为`None`，并将`self.front`向右移动一位，此时`self.front`指向队头元素。
```
    # delete one element from the queue
    def dequeue(self):
        if self.isEmpty():
            raise Exception("Queue is empty")
        else:  
            self.queue[self.front] = None
            self.front += 1
            return self.queue[self.front]
```

* **获取队头元素**：先判断队列是否**为空**，如果为空抛出异常。如果不为空，因为队头指针`self.front`指向队头元素的前一个位置，所以返回`self.front + 1`位置的元素，即`self.queue[self.front + 1]`。

```
    # get the front element of the queue
    def front_val(self):
        if self.isEmpty():
            raise Exception("Queue is Empty")
        else:
            return self.queue[self.front + 1]
```

* **获取队尾元素**：先判断队列是否**为空**，如果为空抛出异常。如果不为空，因为队尾指针`self.rear`指向队尾元素，返回`self.queue[self.rear]`。

```
    # get the rear element of the queue
    def rear_val(self):
        if self.isEmpty():
            raise Exception("Queue is Empty")
        else:
            return self.queue[self.rear]
```

### 2.3 循环顺序队列的实现

* 出队操作总是将队头指针`self.front`向右移动，即`self.front += 1`，入队操作总是在队尾将队尾指针`self.rear`向右移动，即`self.rear += 1`。经过不断的入队、出队操作，整体队列都在向右移动。

* **假溢出**：经过不断的入队、出队操作后，当队尾指针`self.rear == self,size - 1`时，再次进行入队操作会抛出**队列已满**异常，但之前出队操作的空余空间，即从`0`到`self.front - 1`的空间没有利用上。

**解决办法**：
1. 每一次出队操作删除**队头元素**后，把整个队列**向前**移动一个位置。

```
    # delete one element from the queue
    def dequeue(self):
        if self.isEmpty():
            raise Exception("Queue is empty")
        else:  
            val = self.queue[0]

            # 把整个队列向前移动一个位置。
            for i in range(self.rear):
                self.queue[i] = self.queue[i + 1]

            return val
```
* 缺点：
  * 因为队头元素一定在`self.queue[0]`位置，所以队头指针`self.front`几乎用不到
  * 出队操作时间复杂度从`O(1)`变成了`O(n)`

2. 将队列想像成**头尾相连**的循环列表，利用**求摸计算**
* 入队操作：队尾指针循环前进`1`个位置，即`self.rear = (self.rear + 1) % self.size`

* 出队操作：队头指针循环前进`1`个位置，即`self.front = (self.front + 1) % self.size`

**注意**：约定队头指针`self.front`指向队头元素的前一个位置，队尾指针`self.rear`指向队尾元素所在的位置，`self.size`为循环队列可以存储的最大元素个数。当队列初始化时，`self.front == self.rear`；当队列已满时，仍满足`self.front == self.rear`，因此无法分辨此时队列为空，或者队列已满。

**解决办法**：
1. 增加计数器`self.count`，在入队过程中不断更新`self.count`的值
   * 队列**已满**条件：`self.count == self.size`
   * 队列**为空**条件：`self.count == 0`

2. 增加标记变量`self.tag`
   * 队列**已满**条件：因为**入队**操作导致的`self.rear == self.front`，此时`self.tag == 1`
   * 队列**为空**条件：因为**出队**操作导致的`self.rear == self.front`，此时`self.tag == 0`

3. 特意空出来一个位置分辨队列为空或队列已满，即约定队头指针在队尾指针的**下一个**位置作为队列已满的标志。
   * 队列**已满**条件：队头指针在队尾指针的下一个位置，即`(self.rear+ 1) % self.size == self.front`
   * 队列**为空**条件：队头指针等于队尾指针，即`self.front == self.rear`

#### 2.3.1 循环顺序队列的具体实现

按照**方式3**实现循环队列。

* **初始化空队列**：创建一个空队列`self.queue`，定义大小为`self.size + 1`，定义队头元素指针`self.front`和队尾元素指针为`self.rear`都指向`0`。

```
class queue:
    # initilize an empty queue
    def __init__(self, size==100):
        self.queue = [None for _ in range(size + 1)]
        self.size = size + 1
        self.front = 0
        self.rear = 0
```

* **判断队列是否为空**：根据**方式3**，如果队头元素指针`self.front`和队尾元素指针`self.rear`相等，则说明队列为空，否则不为空。

``` 
    # check if the queue is empty
    def isEmpty(self):
        return self.front == self.rear
```

* **判断队列是否为满**：根据**方式3**，队头指针在队尾指针的下一个位置即`(self.rear + 1) % self.size = self.front`，说明队列已满，否则不满。

```
    # check if the queue is full
    def isFull(self):
        return (self.rear + 1) % self.size == self.front
```

* **插入元素（入队）**：即`Enqueue`。先判断队列是否**已满**，如果已满抛出异常。如果没满，`self.rear`向右**循环**移动一位，再进行赋值操作，此时`self.rear`指向队尾元素。
```
    # add one element to the queue
    def enqueue(self, val):
        if self.isFull():
            raise Exception("Queue is full")
        else:
            self.rear = (self.rear + 1) % self.size # 向右循环移动一位
            self.queue[self.rear] = val
```

* **删除元素（出队）**：即`Dequeue`。先判断队列是否**为空**，如果为空抛出异常。如果不为空，`self.front`指向位置赋值为`None`，并将`self.front`向右**循环**移动一位，此时`self.front`指向队头元素。
```
    # delete one element from the queue
    def dequeue(self):
        if self.isEmpty():
            raise Exception("Queue is empty")
        else:  
            self.queue[self.front] = None
            self.front += (self.front + 1) % self.size # 向右循环移动一位
            return self.queue[self.front]
```

* **获取队头元素**：先判断队列是否**为空**，如果为空抛出异常。如果不为空，因为队头指针`self.front`指向队头元素的前一个位置，所以队头元素在队头指针的后一个**循环**位置上，即返回`(self.front + 1) % self.size`位置。

```
    # get the front element of the queue
    def front_val(self):
        if self.isEmpty():
            raise Exception("Queue is Empty")
        else:
            return self.queue[(self.front + 1) % self.size]
```

* **获取队尾元素**：先判断队列是否**为空**，如果为空抛出异常。如果不为空，因为队尾指针`self.rear`指向队尾元素，返回`self.queue[self.rear]`。

```
    # get the rear element of the queue
    def rear_val(self):
        if self.isEmpty():
            raise Exception("Queue is Empty")
        else:
            return self.queue[self.rear]
```

### 2.4 队列链式存储的实现

* 借助于**单链表**来实现**链式队列**。
* 单链表的头节点定义为队头指针`front`，单链表的最后一个节点定义为队尾指针`rear`;注意队头指针`front`指向的是队头元素的前一个位置，即队头元素指向无实际意义的头节点`head`。
* 规定只允许在链表队头进行**删除**操作，即**出队**；只允许在链表队尾进行**插入**操作，即**入队**。

具体实现：
* **链式节点**：构造链式节点`Node`类。

```
#  helper Node class
Class Node:
    def __init__(self, val):
        self.next = None
        self.val = val
```
* **初始化空队列**：建立一个链表头节点`self.head`，定义队头元素指针`self.front`和队尾元素指针`self.rear`都指向头节点，即`self.front = self.front = self.head`。

```
class queue:
    # initilize an empty queue
    def __init__(self):
        self.head = Node(0) # 头节点赋值为0，无实际意义
        self.front = self.head
        self.rear = self.head

```

* **判断队列是否为空**：如果队头元素指针`self.front`和队尾元素指针`self.rear`相等，则说明队列为空，否则不为空。

``` 
    # check if the queue is empty
    def isEmpty(self):
        return self.front == self.rear
```

* **插入元素（入队）**：即`Enqueue`。创建值为`val`的链表节点，插入到链表末尾，并使队尾指针`self.rear`向后移动`1`位，此时队尾指针`self.rear`指向队尾元素。

```
    # add one element to the queue
    def enqueue(self, val):
        node = Node(val)
        self.rear.next = node
        self.rear = node

```

* **删除元素（出队）**：即`Dequeue`。先判断队列是否**为空**，如果为空抛出异常。如果不为空，获取`self.front`下一个节点的值，然后将`self.front`沿链表向后移动一位。如果`self.front`下一个位置等于`self.rear`，说明队列为空，将`self.rear`赋值为`self.front`。返回删除节点的值。
```
    # delete one element from the queue
    def dequeue(self):
        if self.isEmpty():
            raise Exception("Queue is empty")
        else:  
            del_node = self.front.next # 队头元素在队头指针front之后
            self.front.next = del_node.next # self.front位置不变，self.front.next指向删除节点的下一个节点
            if del_node == self.rear:
                self.rear = self.front
            val = del_node.val
            del del_node
            return val
```

* **获取队头元素**：先判断队列是否**为空**，如果为空抛出异常。如果不为空，因为队头指针`self.front`指向队头元素的前一个位置，所以队头元素在队头指针的后一个位置上，即返回`self.front.next.val`。

```
    # get the front element of the queue
    def front_val(self):
        if self.isEmpty():
            raise Exception("Queue is Empty")
        else:
            return self.front.next.val
```

* **获取队尾元素**：先判断队列是否**为空**，如果为空抛出异常。如果不为空，因为队尾指针`self.rear`指向队尾元素，返回`self.rear.val`。

```
    # get the rear element of the queue
    def rear_val(self):
        if self.isEmpty():
            raise Exception("Queue is Empty")
        else:
            return self.rear.val
```

## 3. 队列基础相关题目
| 题号 | 标题 | 标签 | 难度 |
| ----------- | ----------- | ----------- | ----------- |
| 0225| [用队列实现栈 Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/) | 队列、堆栈 | 简单
