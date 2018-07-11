[leetcode链接]https://leetcode.com/problems/single-number-ii/description/  

本题中给出的是一个数组，其中只有一个数出现1次，其余的数都出现3次，题目说要找到只出现1次的数；  
不能再用异或的思想来解这道题；  
解题思路：遍历数组中的所有数字，将每个数组都toBinaryString，统计32bit中各个bit为1的次数，存放到一个32位的int数组sum中；  
将只出现一次的那个数toBinaryString，假设某bit的值为1，那么sum中对应位%3将不为0，否则为0；这样就可以恢复出只出现一次的数字。
（对于出现3次的某个数，将其toBinaryString后，为1的bit位会使得在sum数组中对应的数+3）

#### 普通实现 自己的渣代码实现 排名1.79%： https://leetcode.com/submissions/detail/158843341/

    class Solution {
        public int singleNumber(int[] nums) {
            int[] sums = new int[32];
            for(int num:nums){
                if(num<0){
                    num = (~num) + 1;
                    sums[0] += 1;
                }
                String s = Integer.toBinaryString(num);
                for(int i=0; i<32; i++){
                    if(s.length() <= i) break;
                    int j = s.length() -1 - i;
                    if(s.charAt(j) == '1')
                        sums[31-i] += 1;
                }
            }
            int flag = (sums[0] % 3 ==0 ? 1 : -1);
            int val = 0;
            for(int i=1; i<=31; i++){
                val <<= 1;
                val += (sums[i] % 3);
            }
            if(flag==-1 && val==0)
                return 0x80000000;
            return flag * val;
        }
    }
    
#### 稍微高级一点的实现 排名32.7%

    class Solution {
        public static int singleNumber(int[] nums) {
            int ans = 0;
            for(int i=0; i<32; i++){
                int sum = 0;
                for(int num:nums){
                    if(((num << i) & 0x80000000) == 0x80000000){
                        sum ++;
                    }
                }
                ans <<= 1;
                if(sum % 3 != 0){
                    ans |=  1;
                }
            }
            return ans;
        }
    }

#### 第3种解法 排名68.04%  https://leetcode.com/submissions/detail/158856623/

    class Solution {
        public static int singleNumber(int[] nums) {
            int[] sum = new int[32];
            int ans = 0;
            for(int num: nums){
                for(int j=0; j<32; j++){
                    sum[j] += (num & 1);
                    num >>>= 1;
                }
            }
            for(int i=31; i>=0; i--){
                ans <<= 1;
                ans |= (sum[i] % 3);
            }
            return ans;
        }
    }
    
#### Leetcode上最优解法很简洁，但是没看懂，真是人外有人哇！

