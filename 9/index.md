# 9. 回文数



给你一个整数 `x` ，如果 `x` 是一个回文整数，返回 `true` ；否则，返回 `false` 。


<!--more-->


{{% admonition success "简单" %}}

给你一个整数 `x` ，如果 `x` 是一个回文整数，返回 `true` ；否则，返回 `false` 。

回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。例如，`121` 是回文，而 `123` 不是。

**示例 1：**

    输入：x = 121
    输出：true

**示例 2：**

    输入：x = -121
    输出：false
    解释：从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。

**示例 3：**

    输入：x = 10
    输出：false
    解释：从右向左读, 为 01 。因此它不是一个回文数。

**示例 4：**

    输入：x = -101
    输出：false


**提示：**

* $-2^{31} <= x <= 2^{31} - 1$

{{% /admonition %}}

## 1 解题思路

* 0 为回文数，复数和10的倍数都不是回文数

* 利用[7. 整数反转](https://ieblyang/7/)中的方法将 x 反转
    * 当 x 不为 0 时
        * 将 n 扩大为原来的十倍（相当于每位的数字向左移一位），同时 n 加上 x 的个位数字
        ```n = n * 10 + x % 10;```
        * x 整除 10 （相当于删除掉个位）
    * 当 x 为 0 时结束
        * 若 反转前后大小相等则为回文数 



{{% admonition notes "示例" %}}
以 **示例 1** 为例：
|x|n|while|
|:--:|:--:|:--:|
|begin|0|true|
|121|1|true|
|12|12|true|
|1|121|true|
|0|end|false|

* 反转前后均为121，输出 true

{{% /admonition %}}

## 2 代码实现

```Java
class Solution {
    public boolean isPalindrome(int x) {
        if(x < 0 || x % 10==0) return false;
        if(x == 0) return true;
        int temp = x;
        int n = 0;
        while(x!=0){
            n = n * 10 + x % 10;
            x = x / 10;
        }
        if(temp==n) return true;
        return false;
    }
}
```

## 3 题目链接

<https://leetcode-cn.com/problems/palindrome-number/>
