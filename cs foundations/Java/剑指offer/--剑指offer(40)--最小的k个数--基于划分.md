## [题目]
输入n个整数，找出其中最小的k个数。例如，输入5、4、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4

## [解题思路1]
基于划分partition思想。  
step 1: 每次划分找一个划分的数字t，将小于t的数字移动到数组前一部分，大于t的数字移动到数组后一部分，将t放中间；   
step 2: 如果t正好位于第k个位置，则返回前k个数字，否则继续进行step 1循环;  
总结：注意判断异常输入；注意数组下标；    
代码如下：

    import java.util.ArrayList;
    public class Solution {
        public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
            ArrayList<Integer> al = new ArrayList<Integer>();
            if(k<=0 || k>input.length || input.length==0)
                return al;
            int from=0, to=input.length-1;
            int p = partition(input, from, to);
            k--;
            while(p!=k){
                if(p<k){
                    from = p+1;
                    p = partition(input, from, to);
                }else{
                    to = p-1;
                    p = partition(input, from, to);
                }
            }
            for(int i=0; i<=k; i++){
                al.add(input[i]);
            }
            return al;
        }
        public static int partition(int[] input, int from, int to){
            if(from > to)
                return -1;
            int value = input[from];
            while(from < to){
                while(input[to] >= value && from < to) to--;
                input[from] = input[to];
                while(input[from] < value && from < to) from++;
                input[to] = input[from];
            }
            input[from] = value;
            return from;
        }
    }
