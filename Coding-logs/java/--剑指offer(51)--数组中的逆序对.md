[牛客网本题链接](https://www.nowcoder.com/practice/96bd6684e04a44eb80e6a68efc0ec6c5?tpId=13&tqId=11188&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
题目描述  
在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组,求出这个数组中的逆序对的总数P。并将P对1000000007取模的结果输出。 即输出P%1000000007  
输入描述:  
题目保证输入的数组中没有的相同的数字  

数据范围：  
	对于%50的数据,size<=10^4  
	对于%75的数据,size<=10^5  
	对于%100的数据,size<=2*10^5   
示例1  
输入 1,2,3,4,5,6,7,0
输出 7

解法1 蛮力法  
遍历数组，依次求由各位数作为第一个数，所产生的逆序对个数，时间复杂度o(n^2)，空间复杂度o(1),时间复杂度较高，没能AC  
  public class Solution {
      public int InversePairs(int [] array) {
          if(array==null || array.length <=1)
              return 0;
          int total_inv = 0;
          for(int i=0; i<array.length-1; i++){
              for(int j=i+1; j<array.length; j++){
                  if(array[i] > array[j])
                      total_inv++;
              }
          }
          return total_inv;
      }
  }
  
解法2 
先求数组的每一位起，到数组末尾的最小的数，保存到数组min_num中。然后依次遍历每个数，求其做为第一个数，产生的逆序对个数。如果array[i] < min_num[j],则循环可以终止了。
