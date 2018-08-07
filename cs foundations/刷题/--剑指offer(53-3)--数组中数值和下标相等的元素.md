#### 解法1 蛮力法  
顺序遍历数组，时间复杂度o(n)

  	public static int value_index_equal(int[] array){
		for(int i=0; i<array.length; i++)
			if(i==array[i])
				return array[i];
		return 0x80000000;
	}
  
#### 解法2 基于二分查找的思想1 递归的二分查找

	// 基于二分查找的思想1 -- 基于递归的二分查找
	public static int value_index_equal_2(int[] array, int start, int end){
		if(start > end)
			return -1;
		int mid = (int)((start + end) / 2);
		if(array[mid]==mid)
			return mid;
		else if(array[mid] > mid)
			return value_index_equal_2(array, start, mid-1);
		return value_index_equal_2(array, mid+1, end);
	}
  
#### 解法3 基于二分查找的思想2 -- 基于循环的二分查找

	public static int value_index_equal_3(int[] array){
		  int start = 0, end = array.length-1, mid = (int)((start + end) / 2 );
		  while(start <= end) {
			mid = (int)((start + end) / 2 );
			if(array[mid] == mid) return mid;
			else if(mid < array[mid])
			  end = mid - 1;
			else
			  start = mid + 1;
		  }
		  return -1;
	 }
    
#### 测试

	public static void main(String[] args) {
		int[] array = new int[]{-3,-1,1,2,3,4,5,6,7,8,9, 10,11,12,13,14,15,16,17,18,19,20,21,22,23,
		24,25,26,27,29,32,33,34,35,36,37,38,39,40, 42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,
		  57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80};
		long t1 = System.currentTimeMillis();
		int ans = 0;
		for(int i=0; i<1000000; i++)
		ans = Solution.value_index_equal_2(array, 0, array.length-1);  // 耗时17ms
		//ans = Solution.value_index_equal(array);  // 耗时21ms
		//ans = Solution.value_index_equal_3(array);  // 耗时15ms
		long t2 = System.currentTimeMillis();
		System.out.println(ans+" "+(t2-t1));
	}
