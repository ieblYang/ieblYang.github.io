# 1. 两数之和



给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** 的那 **两个**整数，并返回它们的数组下标。

<!--more-->


{{% admonition success "简单" %}}
给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** 的那 **两个**整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

你可以按任意顺序返回答案。

**示例 1：**

    输入：nums = [2,7,11,15], target = 9
    输出：[0,1]
    解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。

**示例 2：**

    输入：nums = [3,2,4], target = 6
    输出：[1,2]

**示例 3：**

    输入：nums = [3,3], target = 6
    输出：[0,1]


**提示：**

* 2 <= nums.length <= 103
* -109 <= nums[i] <= 109
* -109 <= target <= 109
* 只会存在一个有效答案

{{% /admonition %}}


## 1 解题思路

* 建立空HashMap，其中key为补数，vlaue为下标。 
`HashMap<Integer,Integer> hash = new HashMap<Integer,Integer>();`

* 每遍历nums中一个数`nums[i]`，就去`HashMap`中进行查找，是否存在`key`与`nums[i]`相等

	* 若存在：

		则`nums[i]`为符合条件的整数之一，下标为`i`，另一个整数的下标为`HashMap`中`key=nums[i]`所对应的`value`值；

	* 若不存在：
		
		则将补数 `key = (target - nums[i])` , `value = i` 存入HashMap中；
		`hash.put(target - nums[i]),i);`

{{% admonition notes "示例" %}}
以 **示例 1** 为例：
|i|nums[i]|HashMap|结果|
|:--:|:--:|:--:|:--:|
|0|2|{ 7=0 }|fail|
|1|7|{ 7=0 }|success|

* HashMap中存在`key` = nums[i]，查找成功!
* 第一个下标为`i`= 1，第二个下标为`hash.get(nums[i])` = 0
{{% /admonition %}}

## 2 代码实现

```Java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        // 建立结果数组
        int[] result = new int[2]; 
        
        // 哈希表
        HashMap<Integer,Integer> hash = new HashMap<Integer,Integer>();
        
        for(int i = 0; i < nums.length; i++){
        	// 存在，返回结果
            if(hash.containsKey(nums[i])){
                result[0] = i;
                result[1] = hash.get(nums[i]);
                return result;
            }

            // 不存在，将数据存入
            hash.put(target-nums[i],i);
        }
    
        return result;
    }
}
```

## 3 题目链接

<https://leetcode-cn.com/problems/two-sum/>
