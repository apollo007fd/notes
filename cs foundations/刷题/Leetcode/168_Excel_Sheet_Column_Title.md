168. Excel Sheet Column Title

Given a positive integer, return its corresponding column title as appear in an Excel sheet.

For example:

```
    1 -> A
    2 -> B
    3 -> C
    ...
    26 -> Z
    27 -> AA
    28 -> AB 
    ...
```

**Example 1:**

```
Input: 1
Output: "A"
```

**Example 2:**

```
Input: 28
Output: "AB"
```

**Example 3:**

```
Input: 701
Output: "ZY"
```

思路分析:

显然需要用到求余数, 由测试用例总结出规律.

设映射函数为f(n)

|  n   | 输出 | m=n-1 | 函数f(n) |       规律       |
| :--: | :--: | :---: | :------: | :--------------: |
|  1   |  A   |   0   |   f(1)   |     0%x+'A'      |
|  2   |  B   |   1   |          |     1%x+'A'      |
|  3   |  C   |   2   |          |     2%x+'A'      |
| ...  | ...  |  ...  |   ...    |       ...        |
|  26  |  Z   |  25   |          |     25%x+'A'     |
|  27  |  AA  |  26   |  f(27)   | f(26/x)+26%x+'A' |
|  28  |  AB  |  27   |  f(28)   | f(27/x)+27%x+'A' |
|  29  |  AC  |  28   |  f(29)   | f(28/x)+28%x+'A' |

由于要求余数, 肯定会产生0, 但是n是从1开始计数的, 所以用n-1来计算比较方便.

当n=26时, 输出Z, 显然25%x需要等于25

当n=27时, 输出AA, 显然26%x需要等于0

所以x=26.

```java
class Solution {
    public String convertToTitle(int n) {
        if(n<=0)
            return "";
        n--;
        String res = "";
        int rem = n % 26 + (int)('A');
        int div = n / 26;
        if (div > 0)
            res += convertToTitle(div);
        res += (char)(rem);
        return res;
    }
}
```

