539_Minimum Time Difference 

给出一个list, 其中每个元素都是"hour:minute"格式的时间, 寻找任意一对时间点之间的最小的时间差.

**Example 1:**

```
Input: ["23:59","00:00"]
Output: 1
```



将时间转化为相对于00:00的分钟数, 再用分钟数进行排序, 求相邻的时间点之间, 以及第一个和最后一个时间点时间差. 取最小时间差即可.

```java
class Solution {
    public int findMinDifference(List<String> timePoints) {
        int size = timePoints.size();
        int[] times = new int[size];
        int i = 0;
        for(String s:timePoints)
            times[i++] = toInt(s);
        quickSort(times, 0, times.length-1);
        int mindiff = times[0] + 24*60 - times[size-1];
        for(int j=1; j<size; j++)
            mindiff = Math.min(mindiff, times[j]-times[j-1]);
        return mindiff;
    }
    
    public int toInt(String s){
        String[] ss = s.split(":");
        int ans = Integer.parseInt(ss[1]);
        ans += 60 * Integer.parseInt(ss[0]);
        return ans;
    }
    
    public void quickSort(int[] arrays, int low, int high){
        if(low >= high)
            return;
        int lo = low, hi=high;
        int tmp = arrays[lo];
        while(lo < hi){
            while(lo < hi && arrays[hi] >= tmp) hi--;
            arrays[lo] = arrays[hi];
            while(lo < hi && arrays[lo] <= tmp) lo++;
            arrays[hi] = arrays[lo];
        }
        arrays[lo] = tmp;
        quickSort(arrays, low, lo-1);
        quickSort(arrays, lo+1, high);
    }
}
```



也可以构造boolean数组, 用数组模拟每个时间点.. 效率可能更高一点.