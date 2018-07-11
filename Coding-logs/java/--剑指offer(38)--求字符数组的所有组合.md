# 求字符数组的所有组合
例如：['a', 'b', 'c']的所有组合有a, b, c, ab, ac, bc, abc.

#### [思路]
求数组(长度为n)中从第i个位置起，长为m的组合，0 < m <= n。  
  case 1: 假设结果包含第i个位置，那么需求出第i+1往后长度为m-1的组合；  
  case 2: 假设结果不包含第i个位置，那么需求出第i+1往后长度为m的组合；  
显然可以使用递归来求解。

    import java.util.Stack;
    import java.util.ArrayList;
    public class Solution{
        public static ArrayList<String> solution(String str){
            ArrayList<String> res = new ArrayList<String>();
            if(str==null || str.length()==0)
                return res;
            char[] chars = str.toCharArray();
            Stack<Character> stack = new Stack<Character>();
            for (int i=1; i<=chars.length; i++){
                helper(chars, 0, i, stack, res);
            }
            return res;
        }
        public static void helper(char[] chars, int from, int number, Stack<Character> stack, ArrayList<String> res){
            if(number==0){   //终止条件1
                res.add(stack.toString());
                return;
            }
            if(from+number>chars.length)   //终止条件2
                return;
            stack.push(chars[from]);
            helper(chars, from+1, number-1, stack, res);
            stack.pop();
            helper(chars, from+1, number, stack, res);
        }
        public static void swap(char[] chars, int i, int j){
            char tmp = chars[i];
            chars[i] = chars[j];
            chars[j] = tmp;
        }
    }


