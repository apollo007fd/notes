343_Integer_Break 

给出一个正整数n, 你需要把它分解成若干个(>2)正整数, 这些正整数之和为n, 请问这些正整数的乘积最大是多少?

例如, 给出2, 则答案是1:

2=1+1, 1*1=1.

给出10, 则答案是36:

10 = 3+3+4, 3\*3\*4=36.



求最大和最小, 很明显的DP的题目;

首先定义函数, 假设定义求本问题的函数为F(n).

那么转移状态数组int[] dp的定义为:

$当n<4时:$

$dp[1]=1, dp[2]=2, dp[3]=3;$

当$n>4$时:

$dp[n] = max\{dp[i]*dp[n-i], i=1,2,...,\frac{n}{2}\};  \frac{n}{2}为不大于n的一半的最大整数.$



```
class Solution {
    public int integerBreak(int n) {
        if(n<=2)
            return 1;
        if(n==3)
            return 2;
        
        int[] dp = new int[n+1];
        dp[1]=1; dp[2]=2; dp[3]=3;
        
        for(int i=4; i<=n; i++)
            for(int j=1; j<=i/2; j++)
                dp[i] = Math.max(dp[i], dp[j]*dp[i-j]);
        
        return dp[n];
    }
}
```

