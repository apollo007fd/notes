# 从1到n整数中1出现的次数

从1~n的n个整数中, 1出现的总次数是多少?



1出现的次数 = (个位1出现的次数) + (十位1出现的次数) + ... + (最高位1出现的次数)

以534为例:

1出现的次数 = (个位1出现的次数) + (十位1出现的次数) + ... + (最高位1出现的次数)

​	=(53*1+1) + (5\*10+10) + (0\*100+100) 

f(530) = (53*1+0) + (5\*10+10) + (0\*100 + 100)

f(120) = (12*1+0) + (1\*10+10) + (0\*100 + 20 + 1)



1出现的次数, 就是分别固定个位\十位\ ...\最高位为1, 可能出现的组合个数;

```java
public class Solution {
    public int NumberOf1Between1AndN_Solution(int n) {
        int ans = 0;
        int base1 = 1; //从个位开始
        int ndbase1 = n/base1;
        while(ndbase1!=0){
            ans += (ndbase1/10 * base1); // round * times
            int curbit = ndbase1 % 10;
            if(curbit==1)  //如果最低位为1
                ans += (n%base1+1);
            else if(curbit>1)
                ans += base1;
          
            base1 *= 10;
            ndbase1 /= 10;
        }
        return ans;
    }
}
```

