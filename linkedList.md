# 链表 Linked List

## 1. 链表基础

> 链表 (Linked List): 一种线性表数据结构。使用一组任意的的内存单元，可连续或者不连续，存放相同类型数据。

* 链节点(node)：每个数据元素占用若干存储单元的组合。每个节点包含数据域（存放数据）和指针域（存放后继指针）。
* 后继指针(next pointer)：指出直接后继元素所在的链节点内存地址。逻辑上相邻的两个节点在物理内存地址上可能相邻，也可能不相邻。

### 1.1 链表的优缺点
* **优点**：
  * 存储空间不用事先分配，临时申请动态分配内存，不会造成空间浪费
  * 一些操作的时间效率比数组高（例如：插入，移动，删除等）
* **缺点**：
  * 数据和指针都需要内存空间，空间开销比数组大

### 1.2  链表类型

#### 1.2.1 单链表 Single Linked List
* **头节点（heat node）**：链表的入口节点。
* 最后一个指针域指向**空指针（null）**。

#### 1.2.2 双链表 Double Linked List
* 每个指针域有**两个**指针，分别指向直接前继和直接后继。
* 双链表的每一个节点都可以很方便的访问前继节点和后继节点。

#### 1.2.3 循环链表 Circular Linked List
* 以单链表为基础，最后一个指针指向**头节点**，形成一个环。
* 循环链表的每一个节点出发都可以找到任意节点。
  
### 1.3 链表基本操作

#### 1.3.1 链表结构定义
* 先定义简单链表节点类，即`ListNode`类。
* 然后定义链表类，即`LinkedList`类。`LinkedList`类只有一个头指针`head`表示头节点。
* 链节点由`next`链接。
  
**C/C++**:

```
// Single Linked List
struct ListNode {
    int val; // 节点存储的元素
    ListNode *next; // 后继指针
    ListNode(int x) : val(x), next(NULL) {} // 节点的构造函数
};
```

使用自己定义的构造函数定义节点，在初始化链表的同时赋值：
```
ListNode* head = new ListNode(5);
```

使用默认构造函数初始化节点，然后再赋值：
```
ListNode* head = new ListNode();
head->val = 5
```

**Python**:

```
# ListNode class
class ListNode:
    def __init__(self, val==0, next==None):
        self.val = val
        self.next = next
    
# LinkedList class
class LinekedList:
    def __init__(self):
        self.head = None
```

#### 1.3.2 建立线性链表
* 根据线性表元素动态生成链节点`ListNode`，然后依次链接到链表`LinkedList`中
* 建立一个线性链表的时间复杂度为**O(n)**, **n**为线性表长度。

具体过程：
1. 在所在线性表第`1`个元素开始依次获取元素
2. 每获取一个元素，就生成一个节点，将新节点添加到尾部
3. 插入完毕后返回第`1`个元素的地址

**Python**:
```
# create a linked list based on data
# data: linear list(array)

def create(self, data):
    self.head = ListNode(0) # 定义头节点
    cur = self.head # 当前节点是头节点
    
    for i in range(len(data)):链表
        node = ListNode(data[i]) # 遍历data建立节点node
        cur.next = node # 当前节点的下一个节点为node
        cur = cur.next # 把当前节点改为node，相当于添加到链表尾部
```




