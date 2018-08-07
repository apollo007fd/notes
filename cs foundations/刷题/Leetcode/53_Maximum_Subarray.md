53. Maximum Subarray

给出一个int数组nums, 找出和最大的连续子数组(至少包含一个元素);

**Example:**

```
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6
```

解法:

sum[i] = nums[i], 如果sum[i-1]<=0

否则, sum[i] = sum[i] + nums[i]



```
class Solution {
    public int maxSubArray(int[] nums) {
        int max = Integer.MIN_VALUE;
        int sum = 0;
        for(int i:nums){
            sum = i + (sum > 0 ? sum : 0);
            max = max > sum ? max : sum;
        }
        return max;
    }
}
```

