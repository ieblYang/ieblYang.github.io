# 58. 最后一个单词的长度

给你一个字符串 s，由若干单词组成，单词之间用空格隔开。返回字符串中最后一个单词的长度。如果不存在最后一个单词，请返回 0 。

**单词** 是指仅由字母组成、不包含任何空格字符的最大子字符串。


<!--more-->


{{% admonition success "简单" %}}

给你一个字符串 s，由若干单词组成，单词之间用空格隔开。返回字符串中最后一个单词的长度。如果不存在最后一个单词，请返回 0 。

**单词** 是指仅由字母组成、不包含任何空格字符的最大子字符串。

 

**示例 1：**

    输入：s = "Hello World"
    输出：5

**示例 2：**

    输入：s = " "
    输出：0
 

**提示：**

* `1 <= s.length <= 104`
* `s` 仅有英文字母和空格 `' '` 组成


## 1 解题思路

方法一：使用`split()`函数对所给字符串进行“空格分割”，取出最后一项并求长度

方法二：利用for循环，对所给字符串从后往前遍历，每循环一次`length`加一，当遇到`' '`时停止遍历

{{% /admonition %}}

## 2 代码实现

```Java
//方法一：
class Solution {
    public int lengthOfLastWord(String s) {
        s = s.trim();
        if (s == null || s.isEmpty()) {
            return 0;
        }
        
        String[] strs = s.split(" ");
        return strs[strs.length-1].length();
    }
}

//方法二：
class Solution {
    public int lengthOfLastWord(String s) {
        int length = 0;
        for (int i = s.length() - 1; i >= 0; i--) {
            if (s.charAt(i) != ' ') {
                length++;
            } else if (length != 0) {
                return length;
            }
        }
        return length;
    }
}
```

## 3 题目链接

<https://leetcode-cn.com/problems/length-of-last-word/>

