--剑指offer(61)--扑克牌中的顺子



面试题61：扑克牌中的顺子

题目要求：
抽取5张牌，判断是不是一个顺子。2-10为数字本身，A为1，J为11，Q为12，K为13，大小王可堪称任意数字。



```java
package question61;

public class Solution {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		System.out.println(new Solution().isContinuous(new int[]{1,3,2,5,4}));
	}

    public boolean isContinuous(int [] numbers) {
        // 1.把AJQK、大小王用hashmap映射成数字
    	// 2.大小王是0
    	// 3.将5个数由小到大排序
    	// 4.如果相邻的非零数字相等，则不行，如果间隔大于0的个数，也不行。
    	sort(numbers);
    	int cnt = 0;
    	if(numbers[numbers.length-2]==0) return true;   // 如果倒数第二个数是0，那么最后一个可能是0，也可能非零，这时候都是顺子
    	for(int i=0; i<numbers.length-1; i++){
    		if(numbers[i]==0) 
    			cnt++;
    		else{
    			int diff = numbers[i+1] - numbers[i];
    			if(diff==0)
    				return false;
    			else if(diff==1)
    				continue;
    			else
    				cnt -= (diff-1);
    		}
    	}
    	if(cnt!=0)
    		return false;
    	return true;
    }
    
    public void sort(int[] numbers){
    	for(int i=0; i<numbers.length-1; i++)
    		for(int j=0; j<numbers.length-i-1; j++){
    			if(numbers[j] > numbers[j+1]){
    				int tmp = numbers[j];
        			numbers[j] = numbers[j+1];
        			numbers[j+1] = tmp;
    			}
    		}
    }
}

```

