[牛客网链接](https://www.nowcoder.com/practice/c451a3fd84b64cb19485dad758a55ebe?tpId=13&tqId=11194&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


##### 题目分析  
本题输入一个整数sum,要求寻找所有和为sum的正整数连续序列，序列长度不小于2。  
比如，输入3，输出[1,2]  
输入9，输出[2,3,4],[4,5]  
要找到所有满足要求的正整数连续序列。  


##### 解题思路1：   
假如存在一个长度为i的序列和为sum, 假设该序列第一个数是n,那么该序列之和为 (n+0)+(n+1)+...+(n+i-1)=n*i + (i-1)*i/2;  
所以如果如果sum - (i-1)*i/2=n*i可以被i整除，那么确实存在长度为i的序列满足要求。

    import java.util.ArrayList;
    import java.util.Stack;
    public class Solution {
        public ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {
            ArrayList<ArrayList<Integer>> resList = new ArrayList<ArrayList<Integer>>();
            if(sum <=2 )
                return resList;
            Stack<Integer> num_num = new Stack<Integer>();
            for(int i = 2; true; i++){   // i为序列长度  序列为 min_num+0 、min_num+1 、 ... 、 min_num+(i-1), 序列之和为min_num*i + (i-1)*i/2
                int part_sum = (i*i - i) >> 1;
                if((sum - part_sum) % i == 0){
                    num_num.push(i);
                }
                if(sum - part_sum <= i)   // sum - part_num 已经小于序列的长度，min_num将小于等于0
                    break;
            }
            while(!num_num.isEmpty()){
                int n= num_num.pop();
                int min_num = (sum - ((n*n - n) >> 1)) / n;
                ArrayList<Integer> tmpList = new ArrayList<Integer>();
                for(int i=0; i<n; i++){
                    tmpList.add(min_num+i);
                }
                resList.add(tmpList);
            }
            return resList;
        }
    }

##### 解题思路2：  
用两个数组下标i,j分别指向一个连续正整数序列的两端，假设该序列的第i、j个元素分别表示为a[i],a[j];  
t_sum = a[i]+ a[i+1] + ... + a[j]
如果 t_sum == sum , 则该序列满足要求, 保存该序列，同时i=i+1进入下一次循环；  
如果 t_sum < sum , 则j=j+1;  
如果 t_sum > sum , 则i=i+1;  

    import java.util.ArrayList;
    public class Solution {
        public ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {
            ArrayList<ArrayList<Integer> >  resList = new ArrayList<ArrayList<Integer> >();
            if(sum <=2)
                return resList;
            int i = 1, j=2;
            while(i<j && j<sum){
                int t_sum = (i+j)*(j-i+1) >> 1;
                if(t_sum == sum){
                    ArrayList<Integer> tmp_List = new ArrayList<Integer>();
                    for(int k=i; k<=j; k++)
                        tmp_List.add(k);
                    resList.add(tmp_List);
                    i++;
                }else if(t_sum < sum ){
                    j++;
                }else{
                    i++;
                }
            }
            return resList;
        }
    }

#### 解题思路3 基于思路2   
在解题思路2中，求当前序列的和，可以在上一个序列之和的基础上求和，这样可以减少做乘除运算的次数；  

        import java.util.ArrayList;
        public class Solution {
            public ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {
                ArrayList<ArrayList<Integer> >  resList = new ArrayList<ArrayList<Integer> >();
                if(sum <=2)
                    return resList;
                int i = 1, j=2;
                int t_sum = 1 + 2;
                while(i<j && j<sum){
                    if(t_sum == sum){
                        ArrayList<Integer> tmp_List = new ArrayList<Integer>();
                        for(int k=i; k<=j; k++)
                            tmp_List.add(k);
                        resList.add(tmp_List);
                        t_sum = t_sum - i;
                        i++;
                    }else if(t_sum < sum ){
                        j++;
                        t_sum = t_sum + j;
                    }else{
                        t_sum = t_sum - i;
                        i++;
                    }
                }
                return resList;
            }
        }
