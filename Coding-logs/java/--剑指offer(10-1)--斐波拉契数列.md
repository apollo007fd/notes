# 斐波拉契数列

题目一: 求斐波那契数列的第n项.

​	写一个函数, 输入n, 求斐波拉契(Fibonacci) 数列的第n项. 斐波拉契数列的定义如下:

f(n)  = 0 , n=0

​           1,  n=1

​            f(n-1) + f(n-2), n>1



## 思路1 递归法 

```java
public class Solution {
    public int Fibonacci(int n) {
        if(n<0)
            return 0;
        if(n==0 || n==1)
            return n;
        else
            return Fibonacci(n-1)+Fibonacci(n-2);
    }
}
```



## 思路2 循环法

```java
public class Solution {
    public int Fibonacci(int n) {
        if(n<=0)
            return 0;
        if(n==1)
            return 1;
        int n_1 = 1, n_2=0;
        for(int i=2; i<=n; i++){
            int tmp = n_1 + n_2;
            n_2 = n_1;
            n_1 = tmp;
        }
        return n_1;
    }
}
```

