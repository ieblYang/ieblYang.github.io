# 21. 合并两个有序链表



将两个升序链表合并为一个新的 **升序**链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

<!--more-->


{{% admonition success "简单" %}}

将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

**示例 1：**

    输入：l1 = [1,2,4], l2 = [1,3,4]
    输出：[1,1,2,3,4,4]

**示例 2：**

    输入：l1 = [], l2 = []
    输出：[]

**示例 3：**

    输入：l1 = [], l2 = [0]
    输出：[0]

**提示：**

* 两个链表的节点数目范围是 `[0, 50]`
* `-100 <= Node.val <= 100`
* `l1` 和 `l2` 均按 **非递减顺序** 排列

{{% /admonition %}}

## 1 代码实现

```Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) return l2;
        if (l2 == null) return l1;
        ListNode res = l1.val < l2.val ? l1 : l2;
        res.next = mergeTwoLists(res.next, l1.val >= l2.val ? l1 : l2);
        return res;
    }
}
```

## 2 题目链接

<https://leetcode-cn.com/problems/merge-two-sorted-lists/>
