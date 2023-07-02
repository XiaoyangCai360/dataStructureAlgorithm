# 剑指offer05. 替换空格 Replace Space
[Leetcode 题目链接](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/description/)

## 1. 题目描述
请实现一个函数，把字符串`s`中的每个空格替换成"%20"。

示例1：
```
输入：s = "We are happy."
输出："We%20are%20happy."
```

## 2. 解题思路
* 本题中要求不要浪费额外的空间，基本思想为可以先扩展数组，然后使用 **双指针法** 填充数组
* 字符串和字符数组的区别：字符串可以理解为字符数组，但是字符串提供了更多关于字符处理的相关接口

具体算法：
* 遍历字符串`s`，定义计数器计算空格的数量，扩充字符串大小，增加`2 * count`大小
* 定义双指针`i`和`j`，`i`指向原本字符串结尾，`j`指向新字符串结尾，从后向前填充字符，即如果`s[i]`不是空格，`s[j] = s[i]`，然后`i`和`j`继续向前遍历；如果`s[i]`是空格，`s[j]`、`s[j-1]`和`s[j-2]`分别等于`0`、`2`和`%`
* **从后向前填充**：如果使用从前向后填充，每次填充完一个字符，所有之后的字符都要依次向后移动，时间复杂度为`O(n^2)`；使用从后向前填充就不用，时间复杂度为`O(n)`
* **时间复杂度**：`O(n)`
* **空间复杂度**：`O(1)`

## 3. 算法实现

### 3.1 Python
* `Python`中字符串是不可变类型，需要先把字符串转换为列表，然后填充，空间复杂度不可能为`O(1)`

```Py
class Solution: 
    def replaceSpace(self, s: str) -> str:
        # 扩充字符串
        count = 0

        for c in s:
            if c == ' ':
                count += 1
        
        # str -> list
        s_list = list(s)

        for i in range(2 * count): 
            s_list.append('') 

        # 双指针从后向前填充
        i, j = len(s) - 1, len(s_list) - 1

        while i >= 0:
            if s_list[i] != ' ':
                s_list[j] = s_list[i]
                j -= 1
            else:
                # s_list[i] = ' '
                s_list[j] = '0'
                s_list[j - 1] = '2'
                s_list[j - 2] = '%'
                j -= 3

            i -= 1

        # list -> str
        return ''.join(s_list)
```

### Java
* `Java`中同样定义新列表，然后把字符串转换成列表，之后把列表转换为字符串的时候同样需要`new String(list)`
* 本题中先把字符串转换为列表，然后填充，最后在返回新的字符串，所以空间复杂度也不是`O(1)`

```Java
class Solution {
    public String replaceSpace(String s) {
        int count = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == ' ') {
                count++;
            }
        }

        // String -> list
        // 扩充字符串长度
        char[] sList = new char[s.length() + 2 * count];

        // 填充字符串
        int i = s.length() - 1;
        int j = sList.length - 1;

        while (i >= 0) {
            if (s.charAt(i) != ' ') {
                sList[j] = s.charAt(i);
                j--;
            } else {
                sList[j] = '0';
                sList[j - 1] = '2';
                sList[j - 2] = '%';
                j = j - 3;
            }

            i --;
        }
        
        // list -> String
        return new String(sList);
    }
}
```