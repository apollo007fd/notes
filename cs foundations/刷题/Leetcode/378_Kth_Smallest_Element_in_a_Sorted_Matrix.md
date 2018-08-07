# Kth Smallest Element in a Sorted Matrix 

https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/description/



输入: 一个二维整形数组matrix, 一个整数k; 数组的每一行\列已从小到大排序.

输出: 输出这个数组中第k大的元素.



## 解法1: 小根堆法



## 解法2: 基于值的二分查找法

剑指offer第3题也可以用类似的方法解.

```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        //值的二分查找
        int height=matrix.length, width=matrix[0].length;
        int lo=matrix[0][0], hi=matrix[height-1][width-1];
        int mid, cnt, j;
        while(lo < hi){
            mid = (lo + hi) / 2;
            cnt = 0;
            j = width-1;
            for(int i=0; i<height; i++){
                while(j>=0 && matrix[i][j] > mid) j--;
                cnt += (j+1);
            }
            if(cnt >= k)
                hi = mid;
            else
                lo = mid+1;
        }
        return lo;
    }
}
```

