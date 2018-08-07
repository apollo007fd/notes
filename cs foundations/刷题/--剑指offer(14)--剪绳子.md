# 剪绳子

题目没有给出需要分成多少段, 所以假设最多分成n段.

转移状态矩阵的定义:  int[][] maxmul = new int\[n+1][n+1];

maxmul\[n][m]: 将长度是n的绳子, 分成m段的最大乘积.

$maxmul[n][m+1] = max\{l * maxmul [n-l][m], l \in [1,n-m]\}$

```java
public static int maxMul(int n){
	    int[][] maxmul = new int[n+1][n+1];
	    for(int i=1; i<=n; i++) maxmul[i][1] = i;
	    for(int j=2; j<=n; j++){  // 分成j段
	        for(int i=j; i<=n; i++){   // 总共长度为i,
	            int max = 0;
	            for(int l=1; l<=(i-j+1); l++){  // 第一段长度为l, 剩余长度为(i-l), 分成j-1段, 
	                max = Math.max(max, l*maxmul[i-l][j-1]);
	            }
	            maxmul[i][j] = max;
	        }
	    }
	    int ans = 0;
	    for(int j=2; j<=n; j++)
	        ans = Math.max(ans, maxmul[n][j]);
	    return ans;
	}
```

