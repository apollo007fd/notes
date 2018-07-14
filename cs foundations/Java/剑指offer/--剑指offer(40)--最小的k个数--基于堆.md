## 大根堆
大根堆定义：(小根堆类似)		  
（1）是一棵完全二叉树；  		  
（2）根结点值最大		  
（3）每一棵子树都是大根堆		  
大根堆的操作：建立堆，插入，删除；			  
总结：			  
（1）用数组实现堆时，注意下标的准确表示：			  
当从数组index=0开始存放堆时，index=i的结点，它的左孩子index=2*i+1, 它的右孩子index=2*i+2;	  	  
当从数组index=1开始存放堆时，inex=i的结点，它的左孩子index=2*i, 它的右孩子index=2*i+1;		  
（2）建立大根堆：			  
建堆时，只需从完全二叉树的最后一个非叶子结点为根的子树开始调整。因为叶子结点已经满足大根堆的定义。               
（3）调整子树是一个递归的过程，当根结点与某个孩子结点交换之后，需进一步对该孩子结点的子树进行递归调整。	  	  
[本题牛客网oj](URL 'https://www.nowcoder.com/practice/6a296eb82cf844ca8539b57c23e6e9bf?tpId=13&tqId=11182&tPage=2&rp=2&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking')  
[博客1](URL 'https://www.cnblogs.com/developerY/p/3319618.html')
[博客2](URL 'https://blog.csdn.net/guoweimelon/article/details/50904346')

代码如下：

	import java.util.ArrayList;
	import java.util.Collections;
	public class Solution_heap {
		public static ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
			ArrayList<Integer> al = new ArrayList<Integer>();
			if(input.length==0 || k<=0 || k > input.length)
				return al;
			int[] res = new int[k];
			System.arraycopy(input, 0, res, 0, k);
			buildHeap(res);
			for(int i=k; i<input.length; i++){
				int max = res[0];
				if(max > input[i]){
					res[0] = input[i];
					adjustHeap(res, 0);
				}		
			}
			for(int i: res){
				al.add(i);
			}
			Collections.sort(al);
			return al;
		}
		public static void buildHeap(int[] array){
			for(int i=(array.length-1)/2; i>=0; i--)
				adjustHeap(array, i);
		}
		public static void adjustHeap(int[] array, int i){
			int maxIndex = i;
			if(2*i+1 < array.length && array[2*i+1] > array[maxIndex]){
				maxIndex = 2*i+1;
			}
			if(2*i+2 < array.length && array[2*i+2] > array[maxIndex]){
				maxIndex = 2*i+2;
			}
			if(maxIndex != i){
				swap(array, maxIndex, i);
				adjustHeap(array, maxIndex);
			}
		}
		public static void swap(int[] array, int i, int j){
			int tmp = array[i];
			array[i] = array[j];
			array[j] = tmp;
		}
		public static void main(String[] args) {
			ArrayList<Integer> res = Solution_heap.GetLeastNumbers_Solution(new int[]{4,5,1,6,2,7,3,8}, 4);
			for(int i:res)
				System.out.print(i+" ");
		}
	}
