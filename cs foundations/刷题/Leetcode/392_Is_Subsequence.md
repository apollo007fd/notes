392. Is Subsequence

输入: 字符串s1, s2

功能: 判断s2中是否包含s1, s2中包含的s1的字符在s2中可以不连续, 但是前后要保持一致.

输出: 布尔值



思路解析:

假设s1=[c1, c2, ..., cN] , 假设s1中某个字符c(i-1)匹配成功,  当前要在s2中匹配ci时, s2中可能有多个可以用来匹配ci的字符, 最先出现的, 肯定是一个成功的匹配.



```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        if(s.length()==0)
            return true;
        int i=0, j=0;
        while(j < t.length()){
            if(s.charAt(i)==t.charAt(j)){
                i++;
                if(i==s.length())
                    return true;
            }
            j++;
            if(j==t.length())
                return false;
        }
        return false;
    }
}
```



还可以使用str.indexOf(char c)方法在字符串中查找字符c.