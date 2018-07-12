# [题目]  
输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。  

# 解法一 蛮力法  
求数组元素的全排列，找出全排列中可以组成最小数的那个。  

    import java.util.Arrays;
    import java.util.List;
    import java.util.ArrayList;
    public class Solution {
        public String PrintMinNumber(int [] numbers) {
            if(numbers==null || numbers.length==0)
              return "";
            ArrayList<String> res = new ArrayList<String>();
            permutateHelper(numbers, 0, res);
            Long num = Long.parseLong(res.get(0));  //Integer.parseInt()表示范围不够
            for(String s:res){
              Long tmp = Long.parseLong(s);
              num = num > tmp ? tmp : num;
            }
            return String.valueOf(num);
        }
        //求全排列，是一个递归过程，先把开头的数固定，再对剩下的数求全排列。
        public void permutateHelper(int[] numbers, int from, List<String> lst){
            if(from==numbers.length-1){
              lst.add(toString(numbers));
              return;
            }
            for(int i=from; i<numbers.length; i++){
              swap(numbers, i, from);
              permutateHelper(numbers, from+1, lst);
              swap(numbers, i, from);
            }
        }
        public void swap(int[] numbers, int i, int j){
          int tmp = numbers[i];
          numbers[i] = numbers[j];
          numbers[j] = tmp;
        }
        public String toString(int []numbers){
          String res = "";
          for(int s:numbers)
            res += s;
          return res;
        }
        public static void main(String[] args) {
          Solution solu = new Solution();
          String res = solu.PrintMinNumber(new int[]{3334,3,3333332});
          System.out.println(res);
        }
    }
    
# 解法二 巧妙利用字符串排序
利用Collections.sort()函数对ArrayList进行排序，并自定义Comparator对象中的compare函数，真牛！  

    import java.util.ArrayList;
    import java.util.Collections;
    import java.util.Comparator;
    public class Solution2 {
      public String PrintMinNumber(int [] numbers){
        int n;
        String s="";
        ArrayList<Integer> list = new ArrayList<Integer>();
        n = numbers.length;
        for(int i=0; i<n; i++){
          list.add(numbers[i]);
        }
        Collections.sort(list, new Comparator<Integer>(){
          public int compare(Integer str1, Integer str2){
            String s1 = str1 + "" + str2;
            String s2 = str2 + "" + str1;
            return s1.compareTo(s2);
          }
        });

        for(int j:list){
          s+= j;
        }
        return s;
      }

      public static void main(String[] args) {
        Solution2 sl = new Solution2();
        String res = sl.PrintMinNumber(new int[]{3334,3,3333332});
        System.out.println(res);
      }
    }
