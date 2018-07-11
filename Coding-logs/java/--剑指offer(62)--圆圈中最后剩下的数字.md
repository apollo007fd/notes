--剑指offer(62)--圆圈中最后剩下的数字.md

题意解析：

0~(n-1)这n个数字，围成圆圈，从0开始每次删除第m个数字，求最后剩下的那个数字。



使用循环单链表来实现：空间：o(n)， 时间o(mn)

```java
public class Solution {
    public int LastRemaining_Solution(int n, int m) {
        if(n<=0 || m<=0)
            return -1;
        LinkNode head = new LinkNode(0);
        LinkNode cur = head;
        for(int i=1; i<n; i++){
            cur.next = new LinkNode(i);
            cur = cur.next;
        }
        cur.next = head;
        cur = head;
        while(cur.next != cur){
            for(int i=1; i<m-1; i++){
                cur = cur.next;
            }
            cur.next = cur.next.next;
            cur = cur.next;
        }
        return cur.number;
    }
}

class LinkNode{
    int number = 0;
    LinkNode next = null;
    public LinkNode(int n){
        this.number = n;
    }
}
```

5r