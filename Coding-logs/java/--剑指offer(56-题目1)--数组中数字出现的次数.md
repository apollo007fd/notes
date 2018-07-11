[leetcode链接](https://leetcode.com/problems/single-number-iii/description/)  
[牛客网链接](https://www.nowcoder.com/practice/e02fdb54d7524710a7d664d082bb7811?tpId=13&tqId=11193&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

当只有一个数出现1次，其他数都出现2次是，比较简单。只需将数组中所有的数进行xor操作，就可以得到出现一次的数。  
然而本题有2个出现一次的数。  

思路：  
由于有2个只出现一次的数，因此，需要把数组分为2部分，每一个部分都只包含一个出现一次的数。这样比较麻烦。  
有一种方法巧妙地将数组分为2个部分：先把数组中所有数组做xor操作，求出binary string从右往左数的第一位'1'出现的位置，假设为i。  
再重新遍历数组，根据每个数从右往左第一位'1'出现的位置是否为i，分为2部分。  
2个部分分别进行xor操作，得到2个只出现1次的数。  

同样的思路，自己写的解法比LeetCode最牛解法差了好多啊。

#### 自己写的渣渣解法

    class Solution {
        public int[] singleNumber(int[] array) {
            if(array.length<2)
                return null;

            int nor = 0;
            for(int i : array)
                nor = ( nor ^ i);
            String s = Integer.toBinaryString(nor);
            int j = 0;  // 从右数，第一'1'在第几位
            for(int i=0; i<s.length(); i++){
              j = s.length()-1-i;
              if(s.charAt(j)=='1'){
                j = i;     // j=0,1,2...分别表示从右往左第1个'1'在第1,2,3...位上
                break;
              }
            }
            int[] res = new int[2];
            res[0] = 0;
            res[1] = 0;
            for(int i : array){
              s = Integer.toBinaryString(i); 
              if(s.length() > j && s.charAt(s.length()-1-j) == '1')
                res[0] ^= i;
              else
                res[1] ^= i; 

            }
            return res;
        }
    }

#### LeetCode上的最牛解法， 巧妙求最右边那1位'1'所在的位置

    class Solution {
        public int[] singleNumber(int[] nums) {
            int xor = 0;
            for(int n : nums)
                xor ^= n;

            int lastBit = xor - (xor & (xor - 1)); // n & n-1 會把最右邊的1給消掉
            int a = 0, b = 0;
            for(int n : nums){
                if((n & lastBit) == 0)
                    a ^= n;
                else
                    b ^= n;
            }
            return new int[]{ a, b };
        }
    }

