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
        return self.front == self.size - 1
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

