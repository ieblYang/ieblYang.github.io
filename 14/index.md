# 14. 最长公共前缀



 编写一个函数来查找字符串数组中的最长公共前缀。


<!--more-->


{{% admonition success "简单" %}}

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 `""`

**示例 1：**

    输入：strs = ["flower","flow","flight"]
    输出："fl"


**示例 2：**

    输入：strs = ["dog","racecar","car"]
    输出：""
    解释：输入不存在公共前缀。


**提示：**

* `0 <= strs.length <= 200`
* `0 <= strs[i].length <= 200`
* `strs[i]` 仅由小写英文字母组成

{{% /admonition %}}

## 1 解题思路

* 字符串数组长度为 0 ，返回 `""`
* 字符串数组长度为 1 ，返回 `strs[0]`
* 选择`strs[0]`为初始结果
* 对字符串数组进行遍历
    * 如果 `strs[i]` 不是以 `result` 开始，则让 `result` 变短 

{{% admonition notes "示例" %}}
以 **示例 1** 为例：
|i|strs[i]|result|
|:--:|:--:|:--:|
|0|flower|flower|
|1|flow|flowe|
|1|flow|flow|
|2|flight|flo|
|2|flight|fl|

* 输出结果 `"fl"`

{{% /admonition %}}

## 2 代码实现

```Java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs.length == 0) return "";
       
        if (strs.length == 1) return strs[0];

        String result = strs[0];

        for (int i = 0; i < strs.length; i++) {

            if (!strs[i].startsWith(result)) {
                // 对 result 进行裁剪
                result = result.substring(0, result.length() - 1);
                i--;
            }
        }

        return result;
    }
}
```

## 3 题目链接

<https://leetcode-cn.com/problems/longest-common-prefix/>
