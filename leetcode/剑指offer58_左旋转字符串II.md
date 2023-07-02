# 剑指offer58. 左旋转字符串II Reverse Left Words in a String
[Leetcode 题目链接](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/description/)

## 1. 题目描述
字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。

示例 1：
```
输入: s = "abcdefg", k = 2
输出: "cdefgab"
```

示例 2：
```
输入: s = "lrloseumgh", k = 6
输出: "umghlrlose"
```

## 2. 解题思路
* 本题中如果特别要求不要使用额外空间，只在原本的字符串上操作
* 解题思路和[0151_反转字符串里的单词](/leetcode/0151_%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2%E9%87%8C%E7%9A%84%E5%8D%95%E8%AF%8D.md)相似，使用**整体反转** + **局部反转**的方式反转字符串

具体算法：例：`"abcdefg", n = 2`
* 先反转字符串前`n`个字符（即下标从`0`到`n-1`）；例：`ba + cdefg` 
* 再反转字符串第`n`个字符之后的所有字符（即下标从`n`到`len(s)-1`）；例：`ba + gfedc`
* 最后再反转全部字符串；例：`cdefg + ab`
* **注意**：反转字符串可以使用库函数`reverse`（`Python`中`reversed`）或者自己定义辅助函数；使用库函数`substr`和反转的时间复杂度都是`O(n)`，但是`substr`需要额外空间，空间复杂度不是`O(1)`
* **时间复杂度**：`O(n)`
* **空间复杂度**：`O(1)`

## 3. 算法实现

### 3.1. 列表遍历拼接
Python:
* Python中字符串是不可变类型，需要先把字符串转换为列表，然后填充，空间复杂度为`O(n)`
```Py
class Solution:
    def reverseLeftWords(self, s: str, n: int) -> str:
        # str -> list
        s = list(s)
        n = n % len(s)
        s = s[n:] + s[:n]

        # list -> str
        return "".join(s)
```
Java:
* Java中可以使用`StringBuilder`把不可变的`String`转化为可变的字符序列，相当于使用字符列表；最后把`StringBuilder`转换为`String`用到`StringBuilder.toString()`。
* 使用`StringBuilder`或者把字符串转化为字符列表中都使用了额外空间，空间复杂度为`O(n)`
```Java
class Solution {
    public String reverseLeftWords(String s, int n) {
        // 使用StringBuilder
        StringBuilder res = new StringBuilder();

        // 使用模数优化
        for (int i = n; i < n + s.length(); i++) {
            res.append(s.charAt(i % s.length()));
        }

        // StringBuilder -> String
        return res.toString();
    }
}
```

### 3.2. 自定义反转函数
* 局部反转`[0, n - 1]`，再局部反转`[n, len(s) - 1]`，最后整体字符串反转`[0, len(s) - 1]`

Python:
```Py
class Solution:
    def reverseLeftWords(self, s: str, n: int) -> str:
        # str -> list
        s = list(s)

        # 先反转前n个 [0, n-1]
        self.reverse(s, 0, n - 1)

        # 再反转[n, end]
        self.reverse(s, n, len(s) - 1)

        # 全部反转
        self.reverse(s, 0, len(s) - 1)

        # list -> str
        return "".join(s)

    def reverse(self, s: list, start: int, end: int) -> list:
        while start < end:
            temp = s[start]
            s[start] = s[end]
            s[end] = temp

            start += 1
            end -= 1
```
Java:
```Java
class Solution {
    public String reverseLeftWords(String s, int n) {
        char[] slist = s.toCharArray();
        
        // [0, n-1]
        this.reverse(slist, 0, n - 1);

        // [n, s.length-1]
        this.reverse(slist, n, slist.length - 1);

        // [0, s.length - 1]
        this.reverse(slist, 0, slist.length - 1);

        // char[] -> String
        return new String(slist);
    }

    private char[] reverse(char[] s, int start, int end) {
        while (start < end) {
            char temp = s[start];
            s[start] = s[end];
            s[end] = temp;

            start++;
            end--;
        }

        return s;
    }
}
```