# --剑指offer(60)--n个骰子的点数

抛n个骰子，所有朝上的面的点数之和为sum，求各个值sum出现的概率。

我的写法比较简单，跟书上输出的结果不太一样。总感觉书上写错了。

书上代码的java版：https://www.jianshu.com/p/5fc41ff88c3a

第一种方法，递归法：

f(n, sum) = f(n-1, sum-1) + f(n-1, sum-2) + f(n-1, sum-3) + f(n-1, sum-4) + f(n-1, sum-5) + f(n-1, sum-6);

对于f(n, sum)，终止条件是，当n=1 && sum>=1 && sum<=6, 返回1; 或sum<1, 返回0;

```
public void getCase(int n){
		double sum = Math.pow(6, n);  // 
		for(int i=1*n; i<=6*n; i++)
			System.out.println(i + ":" +new Solution().helper(n, i) * 1.0 / sum);
}

public int helper(int n, int sum){
		if(n==1 && sum <=6 && sum >=1)
			return 1;
		if(sum < 1)
			return 0;
}
```

第二种方法，循环法：

感觉书上写法有问题呢，我觉得每次交换之前，要把当前的数组全部清零。

```java
public void getCase2(int number){
		if(number<=0)
			return ;
		int result[][] = new int[2][6*number+1];
		for(int i=1; i<=6; i++)
			result[1][i] = 1;
		for(int num=2; num<=number; num++){
			for(int j=0; j<6*number+1; j++)
				result[num%2][j] = 0;
			for(int i=num; i<6*num+1; i++){
				for(int j=i-6;j<i;j++){
					if(j>0)
						result[num%2][i] += result[(num-1)%2][j];
				}
			}
		}
		double sum=0;
		for(int i=number;i<6*number+1; i++)
			sum+=result[number%2][i];
		System.out.println("number="+number);
		for(int i=number; i<6*number+1; i++)
			System.out.println("probability "+ i+":"+result[number%2][i]/sum);
	}
```

