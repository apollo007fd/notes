# 正则表达式匹配

请实现一个函数用来匹配包含'.'和'*'的正则表达式. 模式中的字符'.'表示任意一个字符, 而'\*'表示它前面的字符可以出现任意次(含0次).在本题中, 匹配是指字符串的所有字符匹配整个模式.例如, 字符串"aaa"与模式"a.a"和"ab\*ac\*a"匹配, 但与"aa.a"和"ab\*a"均不匹配.

假设原字符串是s1, 模式串是s2.

aaa     和a.a  /  ab*ac\*a匹配



每次从s1中取一个字符, 跟s2的当前状态匹配

如果s2中第2个字符不是*, 则按普通情况处理, 即:

​	如果匹配成功, 则s1和s2分布前进1个字符.

​	如果匹配不成功, 则返回false.

如果s2中第2个字符是*, 则需要分多种情况处理, 即:

​	如果匹配成功, 则分3种情况:

​		s1前进1个字符, s2继续保持当前状态, 继续匹配

​		s1前进1个字符, s2前进2个字符, 继续匹配

​		s1不前进, s2前进2个字符, 继续匹配

​	如果匹配不成功, 则:

​		s1保持当前状态, s2前进2个字符.

```java
public static boolean match(char[] str, char[] pattern){
    if(str==null || pattern==null)
        return false;
    return matchHelper(str, 0, pattern, 0);
}
public static boolean matchHelper(char[] str, int i, char[] pattern, int j){
    if(i==str.length && j==pattern.length)		//匹配完成.
        return true;
    if(i!=str.length && j==pattern.length)       //str还没匹配完, pattern已经用完了.
        return false;
    if(j+1<pattern.length && pattern[j+1]=='*'){  //pattern当前第2个是'*'.
        if(i<str.length && str[i]==pattern[j] || i<str.length && pattern[j]=='.'){   //当前字符匹配成功
            return matchHelper(str, i, pattern, j+2) || matchHelper(str, i+1, pattern, j+2) || matchHelper(str, i+1, pattern, j);
        }
        else      	//当前字符匹配不成功
            return matchHelper(str, i, pattern, j+2);
    }
    if(i<str.length && str[i]==pattern[j] || i<str.length && pattern[j]=='.')    //pattern当前第2个不是'*'
        return matchHelper(str, i+1, pattern, j+1);
    return false;
}
```



```python
# -*- coding:utf-8 -*-
class Solution:
    # s, pattern都是字符串
    def match(self, s, pattern):
        if s==None or pattern==None:
            return False
        return self.matchHelper(s, 0, pattern, 0)
    
    def matchHelper(self, s, i, pattern, j):
        if i==len(s) and j==len(pattern):
            return True
        if i!=len(s) and j==len(pattern):
            return False
        if j+1<len(pattern) and pattern[j+1]=='*':
            if i<len(s) and pattern[j]==s[i] or i<len(s) and pattern[j]=='.':
                return self.matchHelper(s,i,pattern,j+2) or self.matchHelper(s,i+1,pattern,j) or self.matchHelper(s,i+1,pattern,j+2)
            else:
                return self.matchHelper(s,i,pattern,j+2)
        if i<len(s) and pattern[j]==s[i] or i<len(s) and pattern[j]=='.':
            return self.matchHelper(s,i+1,pattern,j+1);
        return False
```

注意python在类中进行函数调用, 被调用函数, 参数第一个必须是self, 调用函数时, 必须用self调用self.funname().



