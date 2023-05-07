# 二分查找 Binary Search Algorithm
> 二分查找过程：
> 1. 从数组中间元素开始，如果元素正好是要查找的元素，查找结束；
> 2. 如果某一元素大于或小于所查找元素，在大于或小于的部分中查找，并从中间比较；
> 3. 如果某一步骤为空，则说明没有找到

## 1. 二分查找重点
- 每一次都通过一些条件判断，排除掉一定不存在目标元素的区间，在剩下可能存在目标元素的区间中继续查找
- 在循环中始终坚持根据查找区间的定义做边界处理，即循环不变量原则，即要在二分查找的过程中，保持不变量，就是在`while`寻找中每一次边界的处理都要坚持根据区间的定义来操作

## 2. 二分查找例题：[704 二分查找 Binary Search](https://leetcode.com/problems/binary-search/)

### 2.1. 题目描述：
给定一个 `n`个元素有序的（升序）整型数组 `nums` 和一个目标值 `target`  ，写一个函数搜索 `nums` 中的 `target`，如果目标值存在返回下标，否则返回`-1`。

- 你可以假设`nums`中的所有元素是不重复的。
- `n`将在`[1, 10000]`之间。
- `nums`的每个元素都将在`[-9999, 9999]`之间。

### 2.2. 前提条件
使用二分法的前提条件：
- 数组`nums`为有序数组。
- 数组`nums`中无重复元素，因为如果有重复元素，返回下标可能不唯一。

### 2.3. 解题思路
二分法的区间定义一般分为两种，**左闭右闭**`[left, right]`或者**左闭右开**`[left, right)`。

### 2.3.1. **左闭右闭**`[left, right]`
- `left = 0, right = len(nums) - 1`。`left`为数组第一个元素，`right`为数组最后一个元素，`[left, right]`上的元素都能取到。
- `if nums[middle] > target`，`right`赋值为`middle - 1`，因为当前`nums[middle] != target`，继续在`[left, middle - 1]`中搜索。
- `if nums[middle] < target`，`left`赋值为`middle + 1`，因为当前`nums[middle] != target`，继续在`[middle + 1, right]`中搜索。
- `middle`取值：`middle = (left + right) // 2`。`Python`中`//`是floor函数，即向下取整。 `middle = left + (right - left) // 2`，为了防止整型溢出。
- 出界条件：`while left <= right`。 一旦退出循坏就意味着目标元素`target`不存在。出界条件为`left = right + 1`, 即`[right, right + 1]`，待查找空间没有元素存在，此时跳出循环返回`-1`。
- 时间复杂度：`O(logn)`；空间复杂度：`O(1)`。

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
        middle = left + (right - left) // 2
        # middle = (left + right) // 2
        
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

### 2.3.2 **左闭右开**`[left, right)`
- `left = 0, right = len(nums)`。`left`为数组第一个元素，`right`不存在，`[left, right)`上的元素能取到`right - 1`。
- `if nums[middle] > target`，`right`赋值为`middle`，因为当前`nums[middle] != target`，继续在`[left, middle）`中搜索，即在下一个区间中不会比较`nums[middle]`。
- `if nums[middle] < target`，`left`赋值为`middle + 1`，因为当前`nums[middle] != target`，继续在`[middle + 1, right]`中搜索。
- `middle`取值：`middle = (left + right) // 2`。`Python`中`//`是floor函数，即向下取整。 `middle = left + (right - left) // 2`，为了防止整型溢出。
- 出界条件：`while left < right`。 一旦退出循坏就意味着目标元素`target`不存在。出界条件为`left = right`, 即`（right, right）`是无效空间，没有意义。循环内`left`可以取到`right - 1`，即数组最后一个元素
- 时间复杂度：`O(logn)`；空间复杂度：`O(1)`。

具体代码实现：
```
class Solution(object):
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
    left, right = 0, len(nums)

    # 在[left, right)区间查找
    # 如果找到就返回
    while left < right:
        middle = left + (right - left) // 2
        # middle = (left + right) // 2
        
        # nums[middle] == target, 返回middle
        if nums[middle] == target:
            return middle
        
            # middle > target -> [left, middle)
        elif nums[middle] > target:
            right = middle
        
            # middle < target -> [middle + 1, right)
        else:
            left = middle + 1

    # 出界就代表没有找到，返回 -1
    return -1
```

## 3. 二分查找相关题目

### 3.1. 有明确`Target`
- 这一类题，会要求找一个`Target`值，一般找到就返回`Target`值的下标或者`Boolean`函数。
- **Medium** 以上题目会对边界选定模糊化，需要先明确边界，再套公式。

| 题号 | 标题 | 标签 | 难度 |
| ----------- | ----------- | ----------- | ----------- |
| 0374 | [猜数字大小 Guess Number Higher or Lower](https://leetcode.com/problems/guess-number-higher-or-lower/) | 二分查找 | 简单 |
| 0367 | [有效的完全平方数 Valid Perfect Squares](https://leetcode.com/problems/valid-perfect-square/) | 二分查找 | 简单 |
| 0033 | [搜索旋转排序数列 Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)| 二分查找 | 中等 |
