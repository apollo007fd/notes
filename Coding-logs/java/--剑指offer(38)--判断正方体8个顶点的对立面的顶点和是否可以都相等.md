### [题目]
输入一个含有8个数字的数组，判断有没有可能把这8个数字分别放到正方体的8个顶点上，使得正方体上三组相对的面上的4个顶点的和都相等。

### [思路]
求8个数字的所有全排列，判断是否包含满足题目要求的全排列。  
8!=40320

    public class Solution{
      private static int c = 0;
      public static boolean judge(int[] arr){
        int[][] permutations = new int[40320][8];
        permutate(arr, 0, arr.length-1, permutations);
        for(int i=0; i<permutations.length; i++){
          int s1 = permutations[i][0]+permutations[i][1]+permutations[i][2]+permutations[i][3];
          int s2 = permutations[i][4]+permutations[i][5]+permutations[i][6]+permutations[i][7];
          int s3 = permutations[i][0]+permutations[i][2]+permutations[i][4]+permutations[i][6];
          int s4 = permutations[i][1]+permutations[i][3]+permutations[i][5]+permutations[i][7];
          int s5 = permutations[i][0]+permutations[i][1]+permutations[i][4]+permutations[i][5];
          int s6 = permutations[i][2]+permutations[i][3]+permutations[i][6]+permutations[i][7];
          if(s1==s2 && s3==s4 && s5==s6)
            return true;
        }
        return false;
      }
      //函数功能: 求arr中从下标from到to的所有全排列
      public static void permutate(int[] arr, int from, int to, int[][] permutations){
        //终止条件
        if(from==to){
          System.arraycopy(arr, 0, permutations[c++], 0, 8);
          return;
        }

        for(int i=from; i<=to; i++){
          swap(arr, from, i);
          permutate(arr, from+1, to, permutations);
          swap(arr, from, i);
        }
      }
      public static void swap(int[] arr, int i, int j){
        int tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
      }
    }
