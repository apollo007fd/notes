# 剑指offer58-2 左旋转字符串

题意分析：

输入一个字符串s和一个数字n，要把这个字符串前n字符，移到s末尾去；



```java
public String LeftRotateString(String str,int n) {
    	if(n > str.length())
    		return "";
    	String newStr = str + str;
    	return newStr.substring(n, n+str.length());
}
```

