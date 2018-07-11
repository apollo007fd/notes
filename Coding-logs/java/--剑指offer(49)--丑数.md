把只包含因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含因子7。 习惯上我们把1当做是第一个丑数。求从小到大第n个丑数。

[leetcode第263题 ugly number](https://leetcode.com/problems/ugly-number/description/)  
[牛客网 丑数](https://www.nowcoder.com/practice/6aa9e04fc3794f68acf8778237ba065b?tpId=13&tqId=11186&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

### 解法一： 蛮力法
由于丑数的因子只能为2、3、5，那么判断一个数是否为丑数可以用一下方法判断：

        class Solution {
            
            public int getUglyNumber(int index){
                if(index <=0)
                    return 0;
                int number = 0;
                int uglyFound = 0;
                while(uglyFound < index){
                    number++;
                    if(isUgly(number))
                        uglyFound++;
                }
                return number;
            }
        
            public boolean isUgly(int num) {
                while(num % 2==0)
                    num /= 2;
                while(num % 3==0)
                    num /= 3;
                while(num % 5==0)
                    num /= 5;
                return num==1;
            }
        }
        
这种解法，时间复杂度较高，每次循环都需要求余数、求商。而且对于不是丑数的数，也需要进行判断。

### 解法二：以空间换时间  
创建数组保存已找到的丑数，用空间换时间的解法。  
这种解法的关键在与创建一个升序的丑数数组，每次都寻找，比当前数组中最大丑数大的那个最小的丑数(下一个待插入数组的丑数)，并插入数组。
很明显，一个丑数乘以2、3或5还是丑数。  
可以用以下方法循环寻找下一个待插入的丑数：  
对于当前丑数数组ua = [1,...,M],(M>=1),ua中最大的一个丑数是M。  
将ua乘以2，并找到大于M的最小的那个数，记为M2;将ua乘以3，并找到大于M的最小的那个数，记为M3；将ua乘以5，并找到大于M的最小的那个，记为M5；
则下一个待插入的数是M2，M3,M5中最小的那个。
循环执行以上方法，直到找到指定的第n个丑数；

        public class Solution {
            public int GetUglyNumber_Solution(int index) {
                if(index<=0) return 0;
                int[] ua = new int[5000]; ua[0] = 1;
                int uglyFound = 1;
                int M = ua[uglyFound-1];
                while(uglyFound < index){
                    int m2=0, m3=0, m5=0, i=0;
                    while(m2 <= M)
                        m2 = ua[i++] * 2;
                    i = 0;
                    while(m3 <= M)
                        m3 = ua[i++] * 3;
                    i=0;
                    while(m5 <= M)
                        m5 = ua[i++] * 5;
                    M = Math.min(m2, m3);
                    M = Math.min(M, m5);
                    ua[uglyFound++]=M;
                }
                return M;
            }

        }
        
### 解法3 解法1的升级版
leetcode上还有一种判断一个数是否为丑数的方法，当除以2时使用整数移位运算，除以3和5的时候依然使用除法运算，移位运算速度更快，相当于解法1的升级版。

        class Solution {
            public boolean isUgly(int num) {
                if(num<=0) return false;
                while(num%2==0)
                    num = num >> 1;
                if(num<=1) return true;
                while(num%3==0)
                    num = num / 3;
                while(num%5==0)
                    num = num / 5;
                return num == 1;
            }
        }
