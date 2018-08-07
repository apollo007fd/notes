## 题目描述

```
在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。请找出数组中任意一个重复的数字。 例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。
```



## 思路1: 使用标记数组

总结: 会使用o(n)的额外空间, 时间复杂度为o(n)



## 思路2: 剑指offer书上解法-基于交换

假设数组中没有重复的数字, 那么可以通过交换, 使得数组下标为i的位置上, 放置数字i,  即arr[i] = i,  (i=0,1,...,n-1).

for i=0,1,...,n-1: 

​	循环交换, 直到将numbers[i]交换到下标numbers[i]处; 如果numbers[numbers[i]]已经等于numbers[i], 则numbers[i]就是重复的数字; 

总计: 会改变数组中的元素. 空间复杂度o(1), 时间复杂度o(n)

```java
public class Solution {
    // Parameters:
    //    numbers:     an array of integers
    //    length:      the length of array numbers
    //    duplication: (Output) the duplicated number in the array number,length of duplication array is 1,so using duplication[0] = ? in implementation;
    //                  Here duplication like pointor in C/C++, duplication[0] equal *duplication in C/C++
    //    这里要特别注意~返回任意重复的一个，赋值duplication[0]
    // Return value:       true if the input is valid, and there are some duplications in the array number
    //                     otherwise false
    public boolean duplicate(int numbers[],int length,int [] duplication) {
        for(int i=0; i<length; i++){
            while(numbers[i] != i){
                // 下标为numbers[i]的元素已经为numbers[i], 
                if(numbers[i] == numbers[numbers[i]]){
                    duplication[0] = numbers[i];
                    return true;
                }
                // 交换
                int tmp = numbers[numbers[i]];
                numbers[numbers[i]] = numbers[i];
                numbers[i] = tmp;
            }
        }
        return false;
    }
}
```



## 区域二分查找

```java
class Solution {
    public int findDuplicate(int[] nums) {
        int n = nums.length-1;
        int low=1, high=n;
        while(low < high){
            int mid = (low+high)/2; //统计落在区间[low,mid]中的数字个数.
            int cnt = 0;
            for(int t:nums){
                if(t>=low && t<=mid)
                    cnt++;
            }
            if(cnt > mid-low+1){
                high=mid;   //前半段有重复数字
            }else
                low =mid+1; //后半段有重复数字
        }
        return low; //最后退出时, 必然是low==high
    }
}
```

