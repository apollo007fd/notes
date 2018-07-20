机器人的运动范围



```java
public class Solution {
    public int movingCount(int threshold, int rows, int cols)
    {
        boolean[][] f = new boolean[rows][cols];
        int[] ans = new int[1];
        visit(f, 0, 0, threshold, ans);
        return ans[0];
    }
    
    public static void visit(boolean[][] f, int i, int j, int k, int[] ans){
        if(i<0 || i>=f.length || j<0 || j>=f[0].length || f[i][j]) return;
        int sum = 0;
        int r = i, c = j;
        while(r != 0){
            sum += r % 10;
            r /= 10;
        }
        while(c != 0){
            sum += c % 10;
            c /= 10;
        }
        if(sum > k) return;
        f[i][j] = true;
        ans[0] += 1;
        visit(f, i-1, j, k, ans);
        visit(f, i+1, j, k, ans);
        visit(f, i, j-1, k, ans);
        visit(f, i, j+1, k, ans);
    }
}
```

