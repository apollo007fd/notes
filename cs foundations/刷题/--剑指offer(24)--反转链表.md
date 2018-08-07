## 反转链表

定义一个函数, 输入一个链表的头节点, 反转该链表并输出反转后链表的头节点. 



**思想**1   递归写法

```python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    # 返回ListNode
    def ReverseList(self, head):
        if head == None or head.next == None:
            return head
        return self.helper(None, head);

    def helper(self, curHead, curNode):
        if curNode == None:
            return curHead
        else:
            nextNode = curNode.next
            curNode.next = curHead
            return self.helper(curNode, nextNode)
```

**思想2**: 循环写法

```python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    # 返回ListNode
    def ReverseList(self, head):
        if head==None or head.next==None:
            return head
        
        newHead = None
        curNode = head
        nextNode = head.next
        
        while curNode != None:
            curNode.next = newHead
            newHead = curNode
            curNode = nextNode
            if curNode != None:
                nextNode = curNode.next
        
        return newHead
```



```
public static ListNode reverseList(ListNode head){
    
}
```

