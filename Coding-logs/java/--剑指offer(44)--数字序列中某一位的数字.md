# 题目类似[LeetCode第400题](https://leetcode.com/problems/nth-digit/description/)  
Find the nth digit of the infinite integer sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ...
Note:
n is positive and will fit within the range of a 32-bit signed integer (n < 2^31).  

# 解法一 蛮力法

# 解法二 巧解 
计算位数<=1位的数字，总共占多少位  
计算位数<=2位的数字，总共占多少位  
......  
计算位数<=k位的数字，总共占多少位  
一共计算到k=10位  
输入n，计算第nth digit属于几位数，假如属于3位数(100~999)，判断属于第几个3位数字，并判断属于该number的第几位，即可求出结果    

    class Solution {
        private int al = 10;
        public int findNthDigit(int n) {
            if(n<=0) return 0;
            if(n<10) return n;
            long[] num_digit_le_k = num_digit_le_k();
            int k=1;
            while(num_digit_le_k[k] < n) k++;
            n -= num_digit_le_k[k-1];   //属于k位number的digit序列的第几个digit
            int num_order = n / k + 1;  //属于k位number的第几个number
            if(n % k==0) num_order = n / k;
            int num_digit = n % k;       //属于该number从左往右数的第几位
            if(n % k == 0) num_digit = k;
            int num = (int)Math.pow(10, k-1) + num_order - 1;  //计算出该number
            num_digit = k+1-num_digit;   //从右往左数的第几位
            int rem=num;
            while(--num_digit != 0){
                rem /= 10;
            }
            return rem%10;
        }
        //求小于等于k(k=1,2,...,al-1)位的数字，共有多少位
        public long[] num_digit_le_k(){
            long[] res = new long[al];
            res[0] = 0; res[1] = 9;
            long sum_num = 10;
            for(int i=2; i<al; i++){
              res[i] = 9 * sum_num;
              sum_num += res[i];
              res[i] *= i;
              res[i] += res[i-1];
            }
            return res;
        }
    }
