矩阵中的路径



a b t g

c f c s

j d e h



遍历矩阵, 从各个位置开始, 分别寻找目标字符串str, 看看是否能寻找成功.

假设str[l-1]匹配成功, 则在str[l-1]在矩阵中的位置周围寻找下一个元素str[l], 如果没有找到str[l], 则回溯, 在str[l-2]周围找另一个str[l-1],  如果寻找失败, 则再回溯, 如果回溯到在str[0]周围找下一个str[1]仍然失败, 则从当前str[0]所在位置开始, 不存在目标字符串. 如果从所有位置开始, 都找不到, 则遍历失败.



```
public class Solution {
    public boolean hasPath(char[] matrix, int rows, int cols, char[] str)
    {
        boolean[][] f = new boolean[rows][cols];
        for(int i=0; i<rows; i++){
            for(int j=0; j<cols; j++)
                if(isPath(matrix, rows, cols, str, 0, i, j, f))
                    return true;
        }
        return false;
    }
    
    //l是当前寻找的字符,从l=0开始
    public boolean isPath(char[] a, int rows, int cols, char[] s, int l, int i, int j, boolean[][] f){
        if(l==s.length)
            return true;
        if(i<0 || i >=rows || j<0 || j>= cols)
            return false;
        if(f[i][j] == false && a[i*cols+j] == s[l]){
            f[i][j] = true;
            if( isPath(a, rows, cols, s, l+1, i-1, j, f) ||   //上
                isPath(a, rows, cols, s, l+1, i+1, j, f) ||   //下
                isPath(a, rows, cols, s, l+1, i, j-1, f) ||   //左
                isPath(a, rows, cols, s, l+1, i, j+1, f)){    //右
                  return true;  
             }else{
                 f[i][j] = false;   //匹配失败,恢复(i,j)为未被占用
             }
        }
        return false;
    }

}
```



回溯法...