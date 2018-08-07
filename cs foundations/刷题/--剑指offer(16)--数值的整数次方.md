# 数值的整数次方

题目: 实现函数double power(double base, int exponent), 求base的exponent次方. 不得使用库函数, 同时不要考虑大数问题.



## 思路1

```java
double power(double base, int exponent){
	//可能的输入: (2.123, -10), (2.123, 0),(2.123, 1),(2.123, 3),(2.123, 8),(2.123, 9),(2.123, 20)
    //先不考虑指数的正负, 负数后面再考虑. 求base的正整数次方.
    if(exponent == 0) return 1.0;
    int count = 1;   // count = 分解后最高的次方+1
    int tmp_exponent = exponent > 0 ? exponent : (-1 * exponent);
    while(tmp_exponnet >= Math.pow(2, count))  count += 1;   //解后最高次方
    double[] components = new double[count];  //记录
    components[1] = base;
    for(int i=2; i<=count; i++)
        components[i] = Math.pow(components[i-1], 2);
    while()
    
}

```

可以考虑: 先将exponent分解成$1, 2, 4, 8, 16, ... 2^n$的和, 然后$base^{expoment}=base^{分解成的指数相加}$. 要求base的高次方,可以用base的低次方求.节省乘法运算次数.



20 = 16 + 4

10 = 8 + 2

9 = 8 + 1

-10 = -8 - 2



7:  0111

------

以上走到了思维误区...按上面这种思想, 应该这样写: 结合位运算. 位分解.

```java
public class Solution {
    public double Power(double base, int exponent) {
        if(base==0)
            return 0;
        double result = 1.0;
        int absExponent = exponent > 0 ? exponent : (-1 * exponent);
        double curBase = base;
        while(absExponent != 0){
            if((absExponent&1)==1)
                result *= curBase;
            curBase *= curBase;
            absExponent = absExponent >> 1;
        }
        if(exponent < 0)
            result = 1.0 / result;
        return result;
    }
}
```





## 思路2

更好的解法, 不是上面这样分解指数的. 而是下面这样:
$$
a^n=
\begin{equation}  
\left\{  
             \begin{array}{**lr**}  
             a^{n/2} * a^{n/2}, & n为偶数&  \\  
             a^{(n-1)/2} * a^{(n-1)/2}*a, & n为奇数\\  
             \end{array}  
\right.  
\end{equation}
$$

```

double power(double base, int exponent){
	if(base == 0)
		return 0;
	int absExponent = exponent > 0 ? exponent : (-1*exponent);
	double ans = powerWithUnsignedExponent(base, absExponent);
	if(exponent < 0)
		ans = 1.0 / ans;
	return ans;
}

double powerWithUnsignedExponent(double base, int exponent){
	if(exponent == 0)
		return 1;
	if(exponent == 1)
		return base;
	double result = powerWithUnsignedExponent(base, exponent);
	result *= result;
	if((exponent & 1) == 1)
		result *= base;
	return result;
}
```

另一种成熟的写法:

```java
public class Solution {
    public double Power(double base, int exponent) {
        // 输入测试用例：
        // 功能测试：（x,+n） (x,-n)
        // 边界测试：（0,0）
        // 负面测试：（0.-n）
        if(equals(base, 0.0))
            return 0;
        if(exponent == 0)
            return 1;
        double result = helpPower(base, exponent>0?exponent:-exponent);
        if(exponent<0){
            result = 1.0/result;
        }
        return result;
    }
        
    private boolean equals(double x, double y){
        if( x-y>-0.0000001 && x-y<0.0000001)
            return true;
        return false;
    }
    
    private double helpPower(double base, int exponent){
        double result=1.0;
        if(exponent==0)
            return 1;
        if(exponent==1)
            return base;
        result *= helpPower(base, exponent>>1);
        result *= result * helpPower(base, exponent&1);
        return result;
    }
}
```

