[牛客网链接](https://www.nowcoder.com/practice/390da4f7a00f44bea7c2f3d19491311b?tpId=13&tqId=11195&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

题目解析：  
输入一个已经递增排序的整形数组array，一个整数s，要求在数组中找到2个数n1,n2，使得n1+n2=s.如果存在多组满足条件的n1,n2，返回一组即可。  

#### 解法1：使用集合
将数组存入一个集合中，再遍历数组，假如当前遍历的数位num，如果s-num也在集合中个，则num和s-sum即是一组满足要求的答案。  
时间复杂度o(n),空间复杂度o(n)  

    import java.util.ArrayList;
    import java.util.HashSet;
    public class Solution {
        public ArrayList<Integer> FindNumbersWithSum(int [] array,int sum) {
            ArrayList<Integer> ans = new ArrayList<Integer>();
            HashSet<Integer> set = new HashSet<Integer>();
            for(int num:array)
                set.add(num);
            for(int num:array)
                if(set.contains(sum - num)){
                    ans.add(num);
                    ans.add(sum - num);
                    break;
                }
            return ans;
        }
    }
    
#### 解法2：前后夹逼法  
定义2个下标索引i,j， 分别指向array第1个、最后一个元素，当i<j时，进行如下循环：  
if array[i] + array[j] == s : 找到，返回；  
else if array[i] + array[j] < s: 那么array[j]肯定不是一个满足要求的数，j--;  
else if array[i] + array[j] > s: 那么array[i]肯定不是一个满足要求的数，i++;  

时间复杂度o(n),空间o(1)  

        import java.util.ArrayList;
        import java.util.HashSet;
        public class Solution {
            public ArrayList<Integer> FindNumbersWithSum(int [] array,int sum) {
                ArrayList<Integer> ans = new ArrayList<Integer>();
                int i=0, j=array.length-1;
                while(i < j){
                    if(array[i] + array[j] == sum){
                        ans.add(array[i]);
                        ans.add(array[j]);
                        return ans;
                    }else if(array[i] + array[j] > sum){
                        j--;
                    }else{
                        i++;
                    }
                }
                return ans;
            }
        }

#### 思维拓展  
假设array是一个无序数组，那么第2种方法就不work了，如果先排序，需要至少o(nlogn)；  
此时第一种方法时、空复杂度仍然没有增加；  
