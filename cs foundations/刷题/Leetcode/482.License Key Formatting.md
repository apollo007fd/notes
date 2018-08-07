You are given a license key represented as a string S which consists only alphanumeric character and dashes. The string is separated into N+1 groups by N dashes.

Given a number K, we would want to reformat the strings such that each group contains *exactly* K characters, except for the first group which could be shorter than K, but still must contain at least one character. Furthermore, there must be a dash inserted between two groups and all lowercase letters should be converted to uppercase.

Given a non-empty string S and a number K, format the string according to the rules described above.

**Example 1:**

```
Input: S = "5F3Z-2e-9-w", K = 4

Output: "5F3Z-2E9W"

Explanation: The string S has been split into two parts, each part has 4 characters.
Note that the two extra dashes are not needed and can be removed.
```

**Example 2:**

```
Input: S = "2-5g-3-J", K = 2

Output: "2-5G-3J"

Explanation: The string S has been split into three parts, each part has 2 characters except the first part as it could be shorter as mentioned above.
```

**Note:**

1. The length of string S will not exceed 12,000, and K is a positive integer.
2. String S consists only of alphanumerical characters (a-z and/or A-Z and/or 0-9) and dashes(-).
3. String S is non-empty.



题意分析：

输入：

一个字符串S：包含数字、字母、破折号"-"，

一个整数K：

要求输出：

将除去破折号的S分成group，每个group不超过K个字符，第一个group可以小于K个字符；



思路:

将S用"-"进行split，划分成字符串数组strs；

将字符串数组strs中的字符串中的字母变成大写，连接起来得到一个长的字符串jointStr；

将jointStr进行子字符串拆分；



将字符串连接的时候，如果不考虑线程同步，使用StringBuilder比较快。

可能的测试用例：

1.全部是字母和数字；

2.包含字母数字和破折号，划分后第一组长度小于K；

3.包含字母数字和破折号，划分后第一组长度等于K；

4.全部都是破折号；



java8中有String.join()方法，可以用来连接字符串数组；

java7中，只能将字符串数组进行手动连接；



```java
public String licenseKeyFormatting(String S, int K) {
    String[] strs = S.split("-");
    StringBuilder jointStr = new StringBuilder();
    for (String s: strs){
    	jointStr.append(s.toUpperCase());
    }
    int len = jointStr.length();
    int firstLen = len % K == 0 ? K : len % K;
    String[] strGroups = new String[(int)Math.ceil(len*1.0/K)];
    strGroups[0] = jointStr.substring(0, firstLen);
    int i = 1;
    while(firstLen < len){
    	strGroups[i++] = jointStr.substring(firstLen, firstLen+K);
    	firstLen += K;
    }
    StringBuilder ans = new StringBuilder(strGroups[0]);
    for(int j=1; j<strGroups.length; j++){
    	ans.append("-");
    	ans.append(strGroups[j]);
    }
    return ans.toString();
}
```
