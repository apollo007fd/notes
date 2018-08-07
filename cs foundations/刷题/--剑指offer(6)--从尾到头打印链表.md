## --剑指offer(6)--从尾到头打印链表

**题目：**输入一个链表的头结点，从尾到头反过来打印出每个结点的值。 

![img](https://images0.cnblogs.com/blog2015/381412/201508/182242237534995.jpg) 

#### 思路1

递归法

终止条件:指针为空;



#### 思路2

使用栈



```
public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ArrayList<Integer> result = new ArrayList<Integer>();
        Stack<ListNode> s = new Stack<ListNode>();
        ListNode pNode = listNode;
        while(pNode != null){
            s.push(pNode);
            pNode = pNode.next;
        }
        while(!s.isEmpty()){
            result.add(s.pop().val);
        }
        return result;
    }
```

