## [问题]
N*N的象棋棋盘，放置N个皇后，不能有两个同行、同列、或在一条斜线上。

## [解法]
创建一个长度为N的数组，放置0~N-1到数组上.数组每个位置放置的数字，相当于对应列中皇后所在的行数；
这样可以使每一列的皇后都不在同一行上。只需遍历所有的排列，找到不在同一斜线上的放置方式即可。

    public class NQueen{
      public static void solution(int n){
        int[] columnIndex = new int[n];
        for(int i=0; i<n; i++)
          columnIndex[i] = i;
        permutation(columnIndex, 0, n-1);
      }
      public static void permutation(int[] columnIndex, int from, int to){
        if(from==to){
          if(judge(columnIndex))
            for(int i=0; i<columnIndex.length; i++)
              System.out.print(""+(columnIndex[i]+1)+" ");
          return;
        }
        for(int i=from; i<=to; i++){
          swap(columnIndex, from, i);
          permutation(columnIndex, from+1, to);
          swap(columnIndex, from, i);
        }
      }
      public static void swap(int[] columnIndex, int i, int j){
        int tmp = columnIndex[i];
        columnIndex[i] = columnIndex[j];
        columnIndex[j] = tmp;
      }
      public static boolean judge(int[] columnIndex){  //判断是否存在皇后共一条斜线(正斜线，反斜线)
        for(int i=0; i<columnIndex.length; i++)
          for(int j=i+1; j<columnIndex.length; j++){
            int row_i = columnIndex[i];
            int row_j = columnIndex[j];
            if(i+row_i == j+row_j)
              return false;
            if(row_i-i == row_j-j)
              return false;
          }
        return true;
      }
    }
