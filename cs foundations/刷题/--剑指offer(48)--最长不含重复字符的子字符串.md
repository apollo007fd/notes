# 剑指offer第48题  
[leetcode 第3题](https://leetcode.com/problems/longest-substring-without-repeating-characters/description/)

## 解法1 蛮力法
分别使用第i个下标做起始字符，求最长满足要求的字符串。时间复杂度最坏情况o(n^3)

        class Solution {
            public int lengthOfLongestSubstring(String s) {
                if(s.equals(""))
                    return 0;
                char[] chars = s.toCharArray();
                int max = 1;
                for(int i=0; i<s.length(); i++){
                    for(int j=i+1; j<s.length(); j++){
                        boolean flag = true;
                        int len = 1;
                        for(int k=i; k<j; k++){
                            if(chars[j]==chars[k]){
                                flag = false;
                                break;
                            }
                            else
                                len++;
                        }
                        if(!flag)
                            break;
                        max = Math.max(len, max);
                    }
                }
                return max;
            }
        }

## 解法2 动态规划  
例如对于字符串“arabcacrf”，对于第i个字符,如果之前没有出现过，则f(i)=f(i-1)+1；  
如果第i个字符串已经出现过，且第i个字符和它上一次出现的位置距离为d:  
    (1）如果d<=f(i-1),则f(i)=d;   
    (2) 如果d>f(i-1),则f(i)=f(i-1)+1;  
设初始f=0;  
使用HashMap存放字符上一次出现的位置下标    

        import java.util.HashMap;
        class Solution {
            public int lengthOfLongestSubstring(String s) {
                if(s.equals(""))
                    return 0;
                HashMap<Character, Integer> last = new HashMap<Character, Integer>();
                int f = 0, max = 1;
                char[] chars = s.toCharArray();
                for(int i = 0; i<chars.length; i++){
                    char c = chars[i];
                    if(!last.containsKey(c)){
                        f = f + 1;
                    }else{
                        int d = i - last.get(c);
                        if(d>f){
                            f = f + 1;
                        }else{
                            f = d;
                        }
                    }
                    max = Math.max(max, f); 
                    last.put(c,i);   
                }
                return max;
            }
        }
