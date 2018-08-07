405_Convert_a_Number_to_Hexadecimal 

给出一个整数num, 编写一个算法, 将它转为16进制数, 对于负整数, 将它的2进制补码转为16进制数;



算法思想:

按正常的解题思路, 需要循环求余数, 做整数除法, 移位; 其实java在内存中将整数当作补码表示, 循环以4bit为单位做位于运算即可.

```java
import java.util.Arrays;
class Solution {
    public String toHex(int num) {
        int lastnum=0;
		int firstNotNull = 0;
		char[] chars = new char[8];
        for(int i=0; i<8; i++){
            lastnum = num & 15;
            if(lastnum<10)
                chars[7-i] = (char)('0'+lastnum);
            else
                chars[7-i] = (char)('a'+lastnum-10);
            num >>= 4;
        }
        while(firstNotNull<7 && chars[firstNotNull]=='0') firstNotNull++;
        return new String(Arrays.copyOfRange(chars, firstNotNull, 8));
    }
}
```

更精简的代码:

```java
class Solution {
    public String toHex(int num) {
        if(num==0) return "0";
        char c;
        char[] hash = new char[]{'0','1','2','3','4','5','6','7','8','9','a','b','c','d','e','f'};
        StringBuilder strbuilder = new StringBuilder();
        while(num!=0){
            c = hash[num & 15];
            strbuilder.append(c);
            num >>>= 4;
        }
        return strbuilder.reverse().toString();
    }
}
```

