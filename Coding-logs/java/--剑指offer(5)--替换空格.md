--剑指offer(5)--替换空格

**题目：**请实现一个函数，把字符串中的每个空格替换成"%20"。例如输入“We are happy.”，则输出“We%20are%20happy.”。　 



题目要求： 实现一个函数，把字符串中的每个空格都替换成“%20”，已知原位置后面有足够的空余位置，要求改替换过程发生在原来的位置上。 



### 思路

遍历一遍数组a,计算字符个数n,空格个数m;从而可以计算出替换后的字符数组的长度n+2m;

从字符数组末尾往前,向后移动字符;

第n个移动到第n+2m个;

遇到空格,插入%20;

```java
public String replaceSpace(StringBuffer str) {
    	int spacenum = 0;
        for(int i=0; i<str.length(); i++){
            if(str.charAt(i)==' ')
                spacenum++;
        }
        int indexold = str.length()-1;
        str.setLength(str.length()+2*spacenum);
        int indexnew = str.length()-1;
        while(indexnew>indexold && indexold>=0){
            if(str.charAt(indexold)==' '){
                str.setCharAt(indexnew--, '0');
                str.setCharAt(indexnew--, '2');
                str.setCharAt(indexnew--, '%');
            }else{
                str.setCharAt(indexnew--, str.charAt(indexold));
            }
            indexold--;
        }
        return str.toString();
    }
```

