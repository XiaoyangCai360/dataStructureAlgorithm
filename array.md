# 数组 Array

### 1. 数组基础

> 数组 (Array)
: 一种线性表数据结构。使用一组连续的内存空间，存放相同类型数据。

* 数组下标从`0`开始 （offset）
* 数组内存地址连续

### 2.  数组基本操作：增，删，改，查

#### 2.1 定义数组       

`Python`: 使用了类似`Java`中的`ArrayList`容器类数据结构，即列表
```
    list1 = []  # 定义空数组
    list2 = list()  # 定义空数组
    list3 = [i for i in range(5)] # list3 = [0,1,2,3,4]
    list4 = [0]*10  # list4 = [0,0,0,0,0,0,0,0,0,0] 明确知道数组长度
```

#### 2.2 查：访问元素

- 访问数组第`i`个元素，只要`i`在合法范围，即`0 <= i <= 数组个数-1`。

- 时间复杂度：`O(1)`。

#### 2.3 增：插入元素
- 数组**尾部**插入：如果数组没有满，在最后插入元素`val`，数组长度不变；如果数组满了，则插入失败。
    - `Python`封装`list.append()`，当数组容量满了，在尾部开辟新的内存空间插入。
    - 时间复杂度：`O（1）`。 
- 数组`i`处插入: 先检查`i`是否合法。确定后，把`i`到`len(list)-1`元素依次向后移动，把`val`插入`i`位置，并更新数组长度。
    - 时间复杂度：移动元素的操作次数与数组长度有关，最坏和平均情况都是`O(n)`。 

#### 2.4 改：改变元素
- 先检查`i`是否合法。确定后，把`i`个元素赋值给`val`。
- 时间复杂度：操作不依赖于数组长度，因此为`O(1)`。 

#### 2.5 删：删除元素
- 数组**尾部**删除：数组长度-1即可。
    - `Python`封装`list.pop()`方法。
    - 时间复杂度：`O（1）`。 
- 数组`i`处插入: 先检查`i`是否合法。确定后，把`i`到`len(list)-1`元素依次向左移动，并更新数组长度。
    - `Python`封装`list.pop(i)`方法。
    - 时间复杂度：移动元素的操作次数与数组长度有关，最坏和平均情况都是`O(n)`。 

### 3. 数组基础题目

#### 3.1 一维数组

| 题号 | 标题 | 标签 | 难度 |
| ----------- | ----------- | ----------- | ----------- |
| 0066 | [加一 Plus One](https://leetcode.com/problems/plus-one/description/) | 数组 | 简单 |
| 0724 | [寻找数组的中心下标 Find Pivot Index](https://leetcode.com/problems/find-pivot-index/) | 数组 | 简单 |
| 0485 | [最大连续1的个数 Max Consecutive Ones](https://leetcode.com/problems/max-consecutive-ones/description/)| 数组 | 简单 |
| 0189 | [轮换数组 Rotate Array](https://leetcode.com/problems/rotate-array/)| 数组 | 中等 |
| 0238 | [除自身以外数组的乘积 Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)| 数组 | 中等 |

### 4.二分查找 Binary Search Algorithm
> 二分查找过程：
> 1. 从数组中间元素开始，如果元素正好是要查找的元素，查找结束；
> 2. 如果某一元素大于或小于所查找元素，在大于或小于的部分中查找，并从中间比较；
> 3. 如果某一步骤为空，则说明没有找到

#### 4.1 二分查找重点
- 每一次都通过一些条件判断，排除掉一定不存在目标元素的区间，在剩下可能存在目标元素的区间中继续查找
- 在循环中始终坚持根据查找区间的定义做边界处理，即循环不变量原则，即要在二分查找的过程中，保持不变量，就是在`while`寻找中每一次边界的处理都要坚持根据区间的定义来操作

#### 4.2 二分查找例题：[704 二分查找](https://leetcode.com/problems/binary-search/)

##### 4.2.1 题目描述：
给定一个 `n`个元素有序的（升序）整型数组 `nums` 和一个目标值 `target`  ，写一个函数搜索 `nums` 中的 `target`，如果目标值存在返回下标，否则返回`-1`。

- 你可以假设`nums`中的所有元素是不重复的。
- `n`将在`[1, 10000]`之间。
- `nums`的每个元素都将在`[-9999, 9999]`之间。

##### 4.2.2 前提条件
使用二分法的前提条件：
- 数组`nums`为有序数组。
- 数组`nums`中无重复元素，因为如果有重复元素，返回下标可能不唯一。

##### 4.2.3 解题思路
二分法的区间定义一般分为两种，**左闭右闭**`[left, right]`或者**左闭右开**`[left, right)`。

##### 4.2.3.1 **左闭右闭**`[left, right]`
- `left = 0, right = len(nums) - 1`。`left`为数组第一个元素，`right`为数组最后一个元素，`[left, right]`上的元素都能取到。
- `if nums[middle] > target`，`right`赋值为`middle - 1`，因为当前`nums[middle] != target`，继续在`[left, middle - 1]`中搜索。
- `if nums[middle] < target`，`left`赋值为`middle + 1`，因为当前`nums[middle] != target`，继续在`[middle + 1, right]`中搜索。
- `middle`取值：`middle = (left + right) // 2`。`Python`中`//`是floor函数，即向下取整。 `middle = left + (right - left) // 2`，为了防止整型溢出。
- 出界条件：`while left <= right`。 一旦退出循坏就意味着目标元素`target`不存在。出界条件为`left = right + 1`, 即`[right, right + 1]`，待查找空间没有元素存在，此时跳出循环返回`-1`。

具体代码实现：
```
class Solution(object):
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
    left, right = 0, len(nums) - 1

    # 在[left, right]区间查找
    # 如果找到就返回
    while left <= right:
        middle = (left + right) // 2
        
        # nums[middle] == target, 返回middle
        if nums[middle] == target:
            return middle
        
            # middle > target -> [left, middle - 1]
        elif nums[middle] > target:
            right = middle - 1
        
            # middle < target -> [middle + 1, right]
        else:
            left = middle + 1

    # 出界就代表没有找到，返回 -1
    return -1
```
##### 4.2.3.2 **左闭右开**`[left, right)`
