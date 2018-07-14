## 题目  
输入一个整形数组，数组里有正数也有负数。数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。要求时间复杂度为O(n)。   

## 解法1  举例分析数组的规律  
遍历数组，记录最大数组和maxSum、当前子数组和curSum：
  在遍历某个数组元素t之前，如果当前子数组和curSum为负数，则将当前子数组curSum和设置为当前遍历的元素值t；否则更新curSum为curSum+t; 在每次遍历完一个元素之后，检查：如果curSum大于maxSum，则更新maxSum为curSum.

    public class Solution {
      public int FindGreatestSumOfSubArray(int[] array) {
        if(array.length==0 || array==null)
          return 0;
        int cursum = 0;
        int maxsum = 0x80000000;  // 最小整数
        for(int i=0; i<array.length; i++){
          if(cursum<0)
            cursum = array[i];
          else
            cursum += array[i];
          if(cursum > maxsum)
            maxsum = cursum;
        }
        return maxsum;
        }
      public static void main(String[] args) {
        Solution sl = new Solution();
        int ans = sl.FindGreatestSumOfSubArray(new int[]{1,-2,3,10,-4,7,2,-5});
        System.out.println(ans);
      }
    }
    
## 解法2 应用动态规划  
使用数组F存储以当前数组元素array[i]结尾的子数组的最大值。  
F[0] = array[0]  
F[i] = max(F[i-1]+array[i], array[i]), for i>0 && i < array.length  
最后求F的最大元素即可  
代码如下：  

    public static int FindGreatestSumOfSubArray(int[] array) {
        int[] maxSum = new int[array.length];
        maxSum[0] = array[0];
        for(int i=1; i<array.length; i++){
          int lastSum = maxSum[i-1];
          maxSum[i] = lastSum >= 0 ? lastSum + array[i] : array[i];
        }
        int max = maxSum[0];
        for (int t:maxSum)
          if(max < t)
            max = t;
        return max;
    }
