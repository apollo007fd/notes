# 合并两个排序的链表

输入两个递增排序的链表, 合并这个两个链表并使新链表中的节点仍然是递增排序的.

![](https://images2015.cnblogs.com/blog/381412/201508/381412-20150830191153219-314974219.jpg)



**思想**1: 

常规解法, 当两个链表都没有遍历结束时, 把当前两个节点中最小的一个加入新链表; 直到一个链表为空, 把另一个链表加入新链表.

代码笔记繁琐.

```
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    # 返回合并后列表
    def Merge(self, pHead1, pHead2):
        if pHead1 == None:
            return pHead2
        if pHead2 == None:
            return pHead1
        head = None
        if pHead1.val < pHead2.val:
            head = pHead1
            pHead1 = pHead1.next
        else:
            head = pHead2
            pHead2 = pHead2.next
        pcur = head
        while pHead1 != None and pHead2 != None:
            if pHead1.val < pHead2.val:
                pcur.next = pHead1
                pHead1 = pHead1.next
            else:
                pcur.next = pHead2
                pHead2 = pHead2.next
            pcur = pcur.next
        while pHead1 != None:
            pcur.next = pHead1
            pcur = pcur.next
            pHead1 = pHead1.next
        while pHead2 != None:
            pcur.next = pHead2
            pcur = pcur.next
            pHead2 = pHead2.next
        return head
```



**思想**2:

使用递归, 代码简洁如下:

```python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    # 返回合并后列表
    def Merge(self, pHead1, pHead2):
        if pHead1 == None:
            return pHead2
        if pHead2 == None:
            return pHead1
        head = None
        if pHead1.val < pHead2.val:
            head = pHead1
            head.next = self.Merge(pHead1.next, pHead2)
        else:
            head = pHead2
            head.next = self.Merge(pHead1, pHead2.next)
        
        return head
```

