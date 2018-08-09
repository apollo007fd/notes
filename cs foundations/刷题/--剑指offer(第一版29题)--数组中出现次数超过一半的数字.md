## 题目描述

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。



### 基于划分的解法--与求数组中第$K$大的数思想类似.

如果长度为$l$的数组中, 存在一个数, 出现次数超过数组长度一半, 即$l/2$, 比如$l=4$时, 出现次数大于2次, $l=5$时, 出现次数大于等于3次.

那么将这个数组从大到小排序, 第$l/2$个(从第0个算起)就是出现次数超过一半的数. 

要求数组中第$l/2$大的数, 相当于求数组中第$k$大的数, 已经有了成熟的$O(n)$算法, 就是基于快排划分的算法.

快排, 对数组进行一次划分, 返回划分点, 则划分点上的数, 已经排到了最终的位置上. 如果这个第$i$点上的数, 小于$k$, 那么说明第$k$大的数, 在$i$之后的位置上, 再对$i$点之后的数组进行划分. 这样递归划分, 直到最终划分点为第$k$点.

```java
public class Solution {
    public int MoreThanHalfNum_Solution(int [] array) {
        int lo=0, hi=array.length-1;
        int pvi = partition(array, lo, hi);
        while(pvi!=array.length/2){  //相当于求数组中第array.length/2+1大的数
            if(pvi<array.length/2)
                lo = pvi+1;
            else
                hi = pvi-1;
            pvi = partition(array, lo, hi);
            if(pvi==-1)
                return 0;
        }
        int obj = array[pvi], cnt=0;
        for(int i:array){
            if(i==obj)
                cnt++;
        }
        if(cnt>array.length/2)
            return obj;
        return 0;
    }
    
    public int partition(int[] array, int lo, int hi){
        if(lo > hi)
            return -1;
        int tmp = array[lo];
        while(lo < hi){
            while(lo < hi && array[hi] >= tmp) hi--;
            array[lo] = array[hi];
            while(lo < hi && array[lo] <= tmp) lo++;
            array[hi] = array[lo];
        }
        array[lo] = tmp;
        return lo;
    }
    
}
```

剑指offer上说这种算法时间复杂度为$O(N)$



### 剑指offer上第二种思想

如果一个数字出现次数超过数组长度的一半, 也就是说这个数出现的次数, 比其他数出现次数的总和还要多.

所以在遍历数组的过程成, 保留一个数以及这个数出现的次数, 每当遍历到这个数时, 出现次数加一, 否则如果遍历到的数字不是这个数, 则出现次数减一, 当次数变为0时, 遍历下一个数, 将下一个数暂存, 将这个数的次数设为1. 则最后暂存的数, 就可能是要找的出现次数大于数组长度一半的数.



```java
public class Solution {
    public int MoreThanHalfNum_Solution(int [] array) {
        int tmp=0, cnt=0;
        for(int n:array){
            if(cnt==0){
                cnt=1;
                tmp=n;
            }else if(tmp==n)
                cnt++;
            else
                cnt--;
        }
        if(cnt==0) return 0;
        cnt = 0;
        for(int n:array)
            if(tmp==n)
                cnt++;
        if(cnt > array.length/2) return tmp;
        return 0;
    }
}
```

