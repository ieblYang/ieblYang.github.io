# 28. 实现 strStr()


实现 `strStr()` 函数。

给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回 `-1`。


<!--more-->


{{% admonition success "简单" %}}

实现 `strStr()` 函数。

给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  `-1`。

**示例 1:**

    输入: haystack = "hello", needle = "ll"
    输出: 2

**示例 2:**

    输入: haystack = "aaaaa", needle = "bba"
    输出: -1

**说明:**

当 `needle` 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 `needle` 是空字符串时我们应当返回 0 。这与C语言的 `strstr()` 以及 Java的 `indexOf()` 定义相符。

{{% /admonition %}}

## 1 解题思路

* 使用 j 进行统计haystack与needle连续相同的数量
* 利用 i 对haystack字符串数组进行遍历
    * 如果 i 所指字符与 j 所指字符**相等**，j 加 1 
        * 如果 j 的长度等于needle的长度，则查找成功，返回`i-j`
    * 如果 i 所指字符与 j 所指字符**不相等**，j 置 0，从`i-j`处重新开始查找
        * 如果haystack剩余的长度小于needle，直接返回 `-1`
* 遍历结束，查找失败，返回 `-1`

{{% admonition notes "示例" %}}
以 **示例 1** 为例：
|j|i|needle.charAt(j)|haystack.charAt(i)|
|:--:|:--:|:--:|:--:|
|0|0|l|h|
|0|1|l|e|
|0|2|l|l|
|1|3|l|l|

* 查找成功，返回 `i-j` ，即2

{{% /admonition %}}

## 2 代码实现

```Java
class Solution {
    public int strStr(String haystack, String needle) {
        if (needle.length() == 0) return 0;
        int len = needle.length();
        int j = 0;
        for(int i =0; i < haystack.length(); i++ ){
            if(haystack.charAt(i)==needle.charAt(j)){
                if (j == len - 1) {
                    return i - j;
                }
                j++;
            } else {
                i = i - j;
                j = 0;
                if (haystack.length() - i < len) {
                    return -1;
                }
            }
        }
        return -1;
    }
}
```

## 3 题目链接

<https://leetcode-cn.com/problems/implement-strstr/>

