--剑指offer(63)--股票的最大利润



题意解析：

给一个数组，每个数字代表一个时刻的股票价格，已按时间顺序排序；

求最大能获得多少利润？

[本题链接](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)

假设在某个时刻卖出股票能获得的最大利润，肯定是之前某个价格最低时刻买入的。

遍历数组，并记录当前时刻之前的股票价格最低值，并记录每个时刻卖出能获得的最大利润。

时间o(n)， 空间o(1)

```java

class Solution {
    public int maxProfit(int[] prices) {
        if(prices==null || prices.length==1)
            return 0;
        int min = 0x7FFFFFFF;
        int max = 0;
        for(int p : prices){
            if(p - min > max)
                max = p - min;
            if(p < min)
                min = p;
        }
        return max;
    }
}
```

