# 链表中倒数第K个节点

输入一个链表, 输出该链表的倒数第K个节点



**思想**1: 遍历2次链表, 第一次得到链表长度, 第二次得到倒数第K个节点

时间复杂度O(N).



**思想**2:快慢指针, 快指针先遍历K个节点, 慢指针才从head开始.

需遍历1次链表, 时间复杂度O(N).



```java
public static ListNode getLastKthNode(ListNode head, int K){
    if(head==null || K <=0)
        return null;
    ListNode quickNode = head;
    ListNode slowNode = head;
    while(K-->1){
        if(quickNode.next==null)
            return quickNode.next;
        quickNode = quickNode.next;
    }
    while(quickNode.next!=null){
        quickNode = quickNode.next;
        slowNode = slowNode.next;
    }
    return slowNode;
}
```

**注意**代码的鲁棒性, 即输入异常处理, 比如输入空链表, K值非法...