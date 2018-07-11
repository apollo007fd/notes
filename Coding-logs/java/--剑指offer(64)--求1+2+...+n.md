--剑指offer(64)--求1+2+...+n



题意解析：

不能用乘除，不能用while, for, if, else , switch, case, 和 A?B:C



不能用循环，就意味着不能用蛮力法;

不能用条件判断，就不能用递归，写不出终止条件;

不能用乘除，就不能用公式。



不使用if，还能使用逻辑与&&来代替if语句；

厉害了，参考：https://www.jianshu.com/p/f77ab41ae245



```java
public static int getSum(int n){
    int t = 0;
    if(n>0)
        t = n + getSum(n-1);
    return t;
}
```

可以写成：

```java
public static int getSum(int n){
    int t = 0;
    boolean b = n>0 && (t = n + getSum(n-1)) > 0;
    return t;
}
```

