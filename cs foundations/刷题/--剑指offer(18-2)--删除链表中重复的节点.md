# 删除链表中重复的节点

题目: 给定一个已经排序的链表, 删除其中重复的节点.例如:

给定:1->2->4->4->5

得到:1->2->5, 4是重复节点



pCur指向新链表中, 待插入的节点.

pLast指向原链表中, 最后一个

```python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    def deleteDuplication(self, pHead):
        # write code here
        if pHead == None:
            return pHead
        pCur = pHead
        pLast = pHead.next
        repeated = False
        while pLast != None:
            if pCur.val == pLast.val:
                repeated=True
                pLast=pLast.next
            else:
                if repeated:
                    pCur.val = pLast.val
                    repeated = False
                    pLast = pLast.next
                else:
                    pCur = pCur.next
                    pCur.val = pLast.val
                    pLast = pLast.next
        pCur.next = None
        #最后一个是否重复
        if not repeated:
            pCur.next = None
        else:
            pNode = pHead
            if pNode.next == None:
                return None
            while pNode.next.next != None:
                pNode = pNode.next
            pNode.next = None
        return pHead
```

