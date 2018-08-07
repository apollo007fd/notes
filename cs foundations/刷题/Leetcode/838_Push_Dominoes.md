838. # Push Dominoes 

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/05/18/domino.png)

**Example 1:**

```
Input: ".L.R...LR..L.."
Output: "LL.RR.LLRRLL.."
```

**Example 2:**

```
Input: "RR.L"
Output: "RR.L"
Explanation: The first domino expends no additional force on the second domino.
```

**Note:**

1. `0 <= N <= 10^5`
2. String `dominoes` contains only `'L`', `'R'` and `'.'`



O(n^2)解法

```java
class Solution {
    public String pushDominoes(String dominoes) {
        char[] doms = dominoes.toCharArray();
        char[] ans = new char[doms.length];
        for(int i=0; i<doms.length; i++){
            // 找左边最近一个L或R的位置
            if(doms[i]!='.'){
                ans[i]= doms[i];
                continue;
            }
                
            int l=i-1;
            while(l>=0 && doms[l]=='.') l--;
            // 找右边最近一个R的位置
            int r=i+1;
            while(r<doms.length && doms[r]=='.') r++;
            
            if(l<0 && r>=doms.length)  //左右两边都是'.'
                ans[i] = '.';
            else if(l<0){   //左边都是'.'
                if(doms[r]=='L')
                    ans[i] = 'L';
                else
                    ans[i] = '.';
            }else if(r>=doms.length){  //右边都是'.'
                if(doms[l]=='R')
                    ans[i] = 'R';
                else
                    ans[i] = '.';
            }else{    //左右两边都有字母
                if(doms[l]=='L' && doms[r]=='R')
                    ans[i] = '.';
                if(doms[l]=='L' && doms[r]=='L')
                    ans[i] = 'L';
                if(doms[l]=='R' && doms[r]=='R')
                    ans[i] = 'R';
                if(doms[l]=='R' && doms[r]=='L'){
                    if(i-l == r-i)
                        ans[i] = '.';
                    else if(i-l < r-i)
                        ans[i] = 'R';
                    else if(i-l > r-i)
                        ans[i] = 'L';
                }
            }
        }
        return new String(ans);
    }
}
```



O(n)解法:

