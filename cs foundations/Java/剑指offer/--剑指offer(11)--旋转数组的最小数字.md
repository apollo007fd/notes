旋转数组的最小数字

3 4 5 1 2     1



| low  | high | mid  | mid > low  | low > mid   |
| ---- | ---- | ---- | ---------- | ----------- |
| 0    | 4    | 2    | low=mid(2) |             |
| 2    | 4    | 3    |            | high=mid(3) |
| 2    | 3    | 2    |            |             |



```
public static int getMin(int[] a){
    if(a==null || a.length==0)
    	return Integer.MIN_VALUE;
    int low = 0, high=a.length-1;
    int mid = low;
    while(a[low] >= a[high]){
        if(high == low + 1){
            mid = high;
            break;
        }
        int mid = (low + high) / 2;
        if(a[mid] >= a[low]){
            low = mid;
        }else{
        	high = mid;
        }
    }
    return mid;
}
```

吐槽, 这题神经病啊!!!! 最后一个情况, 解法不精巧!