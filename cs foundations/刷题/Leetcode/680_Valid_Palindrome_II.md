680_Valid Palindrome II 

给出一个非空字符串s, 可以删除其中某1个字符, 让你判断是否可以将s变为回文字符串.

**Example 1:**

```
Input: "aba"
Output: True
```

**Example 2:**

```
Input: "abca"
Output: True
Explanation: You could delete the character 'c'.
```

**Note:**

1. The string will only contain lowercase characters a-z. The maximum length of the string is 50000.



从s前后分别开始匹配, 假设一次性匹配成功, 则本身就是回文串;

如果在匹配第i和第j个字符的时候, 匹配失败, 则分别从(i+1, j),(i,j-1)开始匹配, 任意一个匹配成功, 就满足要求.

```java
class Solution {
    public boolean validPalindrome(String s) {
        char[] chs = s.toCharArray();
        int i=0, j=s.length()-1;
        for(; i<j; i++, j--)
            if(chs[i] != chs[j])
                break;
        if(i >= j) return true;
        return match(chs, i+1, j) || match(chs, i, j-1);
    }
    
    public boolean match(char[] chs, int i, int j){
        while(i < j)
            if(chs[i++] != chs[j--])
                return false;
        return true;
    }
}
```

