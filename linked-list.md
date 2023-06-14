# 链表 Linked List

## 1. 链表基础

> 链表 (Linked List): 一种线性表数据结构。使用一组任意的的内存单元，可连续或者不连续，存放相同类型数据。

* 链节点(node)：每个数据元素占用若干存储单元的组合。每个节点包含**数据域**（存放数据）和**指针域**（存放后继指针）。
* 后继指针(next pointer)：指出直接后继元素所在的链节点内存地址。逻辑上相邻的两个节点在物理内存地址上可能相邻，也可能不相邻。
* **头指针`*head`**：链表指向**第一个节点**的指针；若链表有**头节点`head`**，则是指向头节点的指针。
* **头节点`head`**：头节点放在第一个**元素节点**之前，其数据域一般**无意义**（也可以用来存放链表的长度），其指针域指向第一个元素节点。
* 无论链表是否为空，**头指针**始终存在。头指针是链表的必要部分，而头节点不是。因此`head->next`后的开始节点才是索引为`0`的节点。

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
  
## 2. 链表基本操作
链表基本操作包括初始化，查找，插入，和删除。

### 2.1 链表结构定义
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
        # 初始化头节点head
        # 头节点head数据为0，无意义
        # 头节点head指针指向None, 空节点
        self.val = val
        self.next = next
    
# LinkedList class
class LinekedList:
    def __init__(self):
        self.head = None
```

### 2.2 链表初始化
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
    self.head = ListNode(0) # 定义头节点，默认初始值为0
    cur = self.head # 新建辅助节点cur在头节点上
    
    for i in range(len(data)):链表
        node = ListNode(data[i]) # 遍历data建立节点node
        cur.next = node # 当前节点的下一个节点为node
        cur = cur.next # 把当前节点改为node，相当于添加到链表尾部
```

### 2.3 求链表的长度
* 链表长度: 链表中包含链表节点的个数，不包含**头节点head**.
* 时间复杂度: **O(n)**。求链表长度需要遍历链表。

具体过程：
1. 定义辅助指针`cur`指向链表的第一个节点。
2. 顺着链节点的`next`遍历链表，辅助指针`cur`每指向一个链节点，计数器`count`做一次计数。
3. 当辅助指针`cur`指向空`None`时，结束遍历，返回计数器`count`数值即为链表长度。

**Python**:
```
# get linked list length
def length(self):
    int count = 0
    cur = self.head

    while cur:
        count += 1
        cur = cur.next
    
    return count
```


### 2.4 链表查找元素
* 在链表中查找数据域为`val`的元素，如果成功就返回查找节点的地址，否则返回`None`。
* 链表不能像数组一样随机访问，只能定义辅助指针`cur`指向头节点`head`，从头节点`head`开始遍历链表，操作问题规模为链表的长度。
* 时间复杂度: **O(n)**。


**Python**:
```
# find element with certain val
def find(self, val):
    cur = self.head # 定义辅助指针指向头节点

    while cur:
        if val == val:
            return cur
        cur = cur.next
    
    return None
```

### 2.5 链表插入元素
链表插入元素分为三种
1. 链表头部插入元素：在链表第一个元素节点前插入数据域为`val`的链节点
2. 链表尾部插入元素：在链表最后一个元素节点之后插入数据域为`val`的链节点
3. 链表中间插入元素：在链表第`i`个元素节点前插入数据域为`val`的链节点

#### 2.5.1 链表头部插入元素
* 时间复杂度: **O(1)**。

算法：
1. 创建一个数据域为`val`的链节点`node`
2. `node`的指针域指向第一个元素节点
3. 头节点`head`的指针指向`node`

**Python**:
```
# insert element in the front of the linked list
def insertFront(self, val):
    node = ListNode(val)
    node.next = self.head
    self.head = node

```

#### 2.5.2 链表尾部插入元素
* 时间复杂度: **O(n)**。
  
算法：
1. 创建一个数据域为`val`的链节点`node`
2. 定义辅助指针`cur`指向头节点`head`
3. 通过`cur.next`遍历链表，直到找到最后一个链节点，即`cur.next = None`
4. `cur.next`指向`node`

**Python**:
```
# insert element in the tail of the linked list
def insertTail(self, val):
    node = ListNode(val)
    cur = self.head # 定义辅助指针

    while cur.next:
        cur = cur.next
    
    cur.next = node
```

#### 2.5.3 链表中间插入元素
* 时间复杂度: **O(n)**。

算法：
1. 定义辅助指针`cur`指向头节点`head`；定义计数器`count`为0。
2. 用`cur.next`遍历指针，指针每指向一个链节点，计数器`count`就做一次计数
3. 当计数器`count`为`index - 1`时，说明此时`cur`遍历到了第`index - 1`个节点，停止遍历
4. 创建一个数据域为`val`的链节点`node`
5. `node.next`指向`cur.next`
6. `cur.next`指向`node`

**Python**:
```
# insert element at certain index of the linked list
def insertAtIndex(self, val, index):
    int count = 0 
    cur = self.head

    while cur:
        count += 1
        cur = cur.next
        if count == index - 1:
            break   # cur是第index - 1个链节点
    
    node = ListNode(val)
    node.next = cur.next
    cur.next = node
```

### 2.6 链表删除元素
链表删除元素分为三种
1. 链表头部删除元素：在链表第一个元素节点前删除数据域为`val`的链节点
2. 链表尾部删除元素：在链表最后一个元素节点之后删除数据域为`val`的链节点
3. 链表中间删除元素：在链表第`i`个元素节点前删除数据域为`val`的链节点

#### 2.6.1 链表头部删除元素
* 时间复杂度: **O(1)**。

算法：
直接将`self.head`向后(右)移动一位。

**Python**:
```
# delete element in the front of the linked list
def deleteFront(self):
    if self.head:
        self.head = self.head.next
```

#### 2.6.2 链表尾部删除元素
* 时间复杂度: **O(n)**。

算法：
1. 创建一个数据域为`val`的链节点`node`
2. 定义辅助指针`cur`指向头节点`head`
3. 通过`cur.next`遍历链表，直到找到**倒数第二个**链节点
4. `cur.next`指向`node`

**Python**:
```
# delete element in the tail of the linked list
def deleteTail(self):
    cur = self.head

    # 出while条件为cur.next.next == None
    # 即cur.next是最后一个节点，cur即为倒数第二个节点
    while cur.next.next: 
        cur = cur.next
        
    cur.next = None
```

#### 2.6.3 链表中间删除元素
* 时间复杂度: **O(n)**。

算法：
1. 定义辅助指针`cur`指向头节点`head`；定义计数器`count`为0。
2. 用`cur.next`遍历指针，指针每指向一个链节点，计数器`count`就做一次计数
3. 当计数器`count`为`index - 1`时，说明此时`cur`遍历到了第`index - 1`个节点，停止遍历
4. `cur.next`指向`cur.next.next`。

**Python**:
```
# delete element at certain index of the linked list
def insertAtIndex(self, index):
    int count = 0 
    cur = self.head

    # 跳出循环时，count = index - 1, cur是index-1个节点
    while cur.next and count < index - 1:
        count += 1
        cur = cur.next
    
    cur.next = cur.next.next
```

## 3. 链表基础操作总结
|  | 头部 | 尾部 | 中间 | 
| ----------- | ----------- | ----------- | ----------- |
| **查找** | O(n) | O(n) | O(n) |
| **插入** | O(1) | O(n) | O(n) | 
| **删除** | O(1) | O(n) | O(n) |

## 4. 链表基础相关题目
| 题号 | 标题 | 标签 | 难度 |
| ----------- | ----------- | ----------- | ----------- |
| 0083| [删除排序链表中的重复元素 Remove Duplicates from Sorted List](https://leetcode.com/problems/remove-duplicates-from-sorted-list/) | 链表 | 简单
| 0203 | [移除链表元素 Remove Linked List Element](https://leetcode.com/problems/remove-linked-list-elements/description/) | 链表 | 简单 |
| 0206 | [反转链表 Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) | 链表 | 简单 |
| 0082| [删除排序链表中的重复元素II Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/description/) | 链表，链表双指针 | 中等
| 0707| [设计链表 Design Linked List](https://leetcode.com/problems/design-linked-list/description/) | 链表 | 中等
