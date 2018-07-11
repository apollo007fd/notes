扫描在一个整型数组array中，整数k出现的次数.  
[本题牛客网链接](https://www.nowcoder.com/practice/70610bf967994b22bb1c26f9ae901fa2?tpId=13&tqId=11190&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)  
总结：二分查找有2种写法，一种是循环写法，一种是递归写法，时间复杂度是o(logn)  
#### 解法1 蛮力法  
从头到尾遍历数组，当遇到比k小的数时，continue. 当遇到等于k的数时，计数加1. 当遇到大雨k的数时，遍历终止.  
代码：  

    public class Solution {
        public int GetNumberOfK(int [] array , int k) {
            if(array.length==0 || array==null)
                return 0;
            int cnt = 0;
            for(int i=0; i<array.length; i++){
                if(array[i] < k)
                    continue;
                if(array[i] > k)
                    break;
                if(array[i]==k)
                    cnt++;
            }
            return cnt;
        }
    }

#### 解法2 二分查找法  
先用二分查找，找到一个k，这时这个k的左右两边都可能存在k,从左边的数组尾往前计数k的个数得到cnt1, 从右边的数组头往后计数k的个数，得到cnt2; 返回cnt1+cnt2.  

    public class Solution {
        public int GetNumberOfK(int [] array , int k) {
            if(array.length==0 || array==null)
                return 0;
            int i=0, j=array.length-1, mid=0;
            while(i <= j){
                mid = (int)(0.5*i + 0.5*j);
                if(array[mid] == k)
                    break;
                else if(array[mid] < k)
                    i = mid + 1;
                else
                    j = mid-1;
            }
            if(array[mid]!=k)
                return 0;
            int cnt = 1;
            for (int l=mid-1; l>=i; l--)
                if(array[l] != k)
                    break;
                else
                    cnt++;
            for (int l=mid+1; l<=j; l++)
                if(array[l] != k)
                    break;
                else
                    cnt++;
            return cnt;
        }
    }
    
#### 解法3 找第一个k、最后一个k的位置  
用二分法查找第一个k的出现位置，再用二分法查找最后一个k的出现位置，即可求得答案。  
二分查找时间复杂度是o(logn), 因此总的复杂度也只有o(logn)  

    public class Solution {
        public int GetNumberOfK(int [] array , int k) {
            if(array==null || array.length==0)
                return 0;
            int start = this.getFirst(array, k, 0, array.length-1);
            int end = this.getLast(array, k, 0, array.length-1);
            if(start ==-1 && end==-1)
                return 0;
            return end - start + 1;
        }

        public int getFirst(int[] array, int k, int i, int j){
            int mid=(int)((i+j)/2);
            while(i <= j){
                mid = (int)((i+j)/2);
                if(array[mid] == k)
                    if( mid==0 || array[mid-1]!=k){
                        return mid;
                    }
                    else
                        j = mid -1;
                else if(array[mid] > k)
                    j = mid - 1;
                else
                    i = mid + 1;
            }
            return -1;
        }

        public int getLast(int[] array, int k, int i, int j){
            int mid=(int)((i+j)/2);
            while(i <= j){
                mid = (int)((i+j)/2);
                if(array[mid] == k)
                    if(mid==array.length-1 || array[mid+1]!=k){
                        return mid;
                    }
                    else
                        i = mid + 1;
                else if(array[mid] > k)
                    j = mid - 1;
                else
                    i = mid + 1;
            }
            return -1;
        }
    }
