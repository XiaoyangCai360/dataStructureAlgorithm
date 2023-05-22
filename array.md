# 数组 Array

### 1. 数组基础

> 数组 (Array): 一种线性表数据结构。使用一组连续的内存空间，存放相同类型数据。

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
| 1822 | [数组元素积的符号 Sign of the Product of an Array](https://leetcode.com/problems/sign-of-the-product-of-an-array/description/)| 数组 | 简单 |
| 2215 | [找出两数组的不同 Find the Difference of Two arrays](https://leetcode.com/problems/find-the-difference-of-two-arrays/description/)| 数组 | 简单
| 0349 | [两个数组的交集 Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/description/)| 数组 | 简单
| 0189 | [轮换数组 Rotate Array](https://leetcode.com/problems/rotate-array/)| 数组 | 中等 |
| 0238 | [除自身以外数组的乘积 Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)| 数组 | 中等 |
