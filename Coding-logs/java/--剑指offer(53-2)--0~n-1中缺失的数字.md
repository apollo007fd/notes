[leetcode本题链接](https://leetcode.com/problems/missing-number/description/)  
本题在剑指offer和LeetCode上略有不同，但是不影响解题策略的选择，本解析以LeetCode为准；  

#### 解法1 蛮力法  49.48%

    class Solution {
        public int missingNumber(int[] nums) {
            boolean[] flag = new boolean[nums.length+1];
            for(int t:nums)
                flag[t] = true;
            for(int i=0; i<flag.length; i++){
                if(flag[i] == false)
                    return i;
            }
            return -1;
        }
    }

改进：
蛮力法要遍历2次数组，时间复杂度为o(n); 使用了一个辅助数组，空间复杂度为o(n)。  
LeetCode上不是排序数组，如果是排序数组，比较好解；只需对比每一段数组中下标差和数值差是否相同即可。可以做到时间复杂度为o(logN)。
对于排序数组，剑指offer上的解法，只需对比数组值与下标是否相同。  
对于非排序数组，如果先排序，排序时间复杂度最少是o(nlogn), 比解法1还差。  

#### 解法2  leetcode最优解法  


思路说明：  
假设数组中的数字是0-n,缺k; 数组下标是0-(n-1),那么数组中所有数字之和为sum = {0+1+2+...+(n-1)+n}-k，下标之和为sum_index = 0+1+2+...+(n-1).  
sum - sum_index = n-k  
那么，(sum_index+n) - sum = k;  
代码如下：  

    class Solution {
        public int missingNumber(int[] nums) {
            int sum_index = nums.length;
            for(int i =0; i<nums.length; i++){
                sum_indx = sum_index - nums[i] + i;
            }
            return sum;
        }
    }
