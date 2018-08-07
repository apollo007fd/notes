350. Intersection of Two Arrays II 

题意:

输入2个数组, 数组是无序的, 找到2个数组的交集. 

- Each element in the result should appear as many times as it shows in both arrays.

```java
class Solution {
    public static int[] intersect(int[] nums1, int[] nums2) {
        quickSort(nums1, 0, nums1.length-1);
        quickSort(nums2, 0, nums2.length-1);
        int[] ans = new int[nums1.length > nums2.length ? nums1.length : nums2.length];
        int n=0;
        for(int i=0,j=0; i<nums1.length && j<nums2.length;){
			if(nums1[i]==nums2[j]){
                ans[n++]=nums1[i];
				i++; j++;
			}else if(nums1[i] < nums2[j])
				i++;
			else
				j++;
		}
		return Arrays.copyOfRange(ans, 0, n);
    }
    
    public static void quickSort(int[] nums, int begi, int endi){
        if(begi >= endi)
            return;
        int i=begi, j=endi;
        int tmp = nums[i];
        while(i<j){
            while(j > i && nums[j]>=tmp) j--;
            nums[i] = nums[j];
            while(i < j && nums[i]<=tmp) i++;
            nums[j] = nums[i];
        }
        nums[i] = tmp;
        quickSort(nums, begi, i-1);
        quickSort(nums, i+1, endi);
    }
}
```

