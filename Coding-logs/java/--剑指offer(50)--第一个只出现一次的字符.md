## 题目一:  
在一个字符串(1<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置  
本题OJ链接：  
[牛客网链接](https://www.nowcoder.com/practice/1c82e8cf713b4bbeb2a5b31cf5b0417c?tpId=13&tqId=11187&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)  
[Leetcode链接](https://leetcode.com/problems/first-unique-character-in-a-string/description/)  

解法1 使用哈希表存储字符出现的次数、第一次出现的次数  
由于字符char是一个长度为8的数据类型，因此总共有256种可能。可以用数组来表示一个简单的哈希表。  
使用2个辅助数组分别存储出现次数、第一次出现位置; 再遍历辅助数组，找出第一个出现次数为1的字符  
时间复杂度o(n), 空间复杂度o(1)  

        class Solution {
            public int firstUniqChar(String s) {
                if(s==null || s.length()==0)
                    return -1;
                int[] time = new int[256];
                int[] first = new int[256];
                char[] chars = s.toCharArray();
                for(int i=0; i<chars.length; i++){
                    char c = chars[i];
                    time[(int)c] += 1;
                    if(first[(int)c]==0)
                        first[(int)c] = i;
                }
                int min = 0x7FFFFFFF;
                for(int i=0; i<time.length; i++){
                    if(time[i]==1 && first[i] < min){
                        min = first[i];
                    }
                }
                if(min==0x7FFFFFFF) min=-1;
                return min;
            }
        }

相关的问题：  
1.输入2个字符串，从第一个字符串中删除在第二个字符串中出现过的所有字符  
2.删除字符串中所有重复出现的字符  
3.在英文中，如果2个单词中初始的字母相同，并且每个字母出现的次数也相同，那么这2个单词互为变位词。例如，silent与listen、evil与live互为变位词。请完成一个函数，判断输入的两个单词是否互为变位词。  

## 题目二:  
题目描述：  
请实现一个函数用来找出字符流中第一个只出现一次的字符。例如，当从字符流中只读出前两个字符"go"时，第一个只出现一次的字符是"g"。当从该字符流中读出前六个字符“google"时，第一个只出现一次的字符是"l"。  
输出描述:  
如果当前字符流没有存在出现一次的字符，返回#字符。  

    public class Solution {
        //Insert one char from stringstream
        public int index;
        public int[] occurrence = new int[256];

        public Solution(){
            for(int j=0; j<occurrence.length; j++)
                occurrence[j] = -1;
            index = 0;
        }

        public void Insert(char ch)
        {
            if(occurrence[(int)ch]==-1)
                occurrence[(int)ch] = index;
            else if(occurrence[(int)ch]>=0)
                occurrence[(int)ch] = -2;
            index++;
        }
      //return the first appearence once char in current stringstream
        public char FirstAppearingOnce()
        {
            int min = 0x7FFFFFFF;
            char res = '#';
            for(int i=0; i<256; i++){
                if(occurrence[i]>=0 && min > occurrence[i]){
                    min = occurrence[i];
                    res = (char)i;
                }
            }
            return res;
        }
    }
