# 35. 搜索插入位置


给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。


<!--more-->


{{% admonition success "简单" %}}

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

你可以假设数组中无重复元素。

**示例 1:**

    输入: [1,3,5,6], 5
    输出: 2

**示例 2:**

    输入: [1,3,5,6], 2
    输出: 1

**示例 3:**

    输入: [1,3,5,6], 7
    输出: 4

**示例 4:**

    输入: [1,3,5,6], 0
    输出: 0

{{% /admonition %}}

## 1 解题思路

* 使用二分查找即可解决

{{% admonition notes "示例" %}}
以 **示例 1** 为例：
|left|right|mid|nums[mid]|
|:--:|:--:|:--:|:--:|
|0|3|1|3|
|2|3|2|5|


* 查找成功，返回 left ，即 2

{{% /admonition %}}

## 2 代码实现

```Java
class Solution {
    public int searchInsert(int[] nums, int target) {

        int right = nums.length-1;
        int left = 0;

        while(left<=right){

            int mid = (left+right)/2;

            if(target == nums[mid]){
                return mid;
            }

            if(target > nums[mid]){
                left = mid+1;
            }

            if(target < nums[mid]){
                right = mid-1;
            }
        }
        return left;
       
    }
}
```

## 3 题目链接

<https://leetcode-cn.com/problems/search-insert-position/>

