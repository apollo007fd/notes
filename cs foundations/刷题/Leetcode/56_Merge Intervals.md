# 56_Merge Intervals 

给出一组区间, 合并有重合的区间.

**Example 1:**

```
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
```

**Example 2:**

```
Input: [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considerred overlapping.
```



区间表示为[start, end];

按所有区间的start 对区间进行排序.

然后,  使用last记录当前最后一个区间的end, 对于下一个区间, 如果有重合就合并, 否则添加一个新的区间.

```
/**
 * Definition for an interval.
 * public class Interval {
 *     int start;
 *     int end;
 *     Interval() { start = 0; end = 0; }
 *     Interval(int s, int e) { start = s; end = e; }
 * }
 */
import java.util.ArrayList;
import java.util.Collections;
class Solution {
    public List<Interval> merge(List<Interval> intervals) {
        List<Interval> list = new ArrayList<Interval>();
        Comparator<Interval> comparator = new Comparator<Interval>(){
            public int compare(Interval o1, Interval o2){
                return o1.start - o2.start;
            }
        };
        Collections.sort(intervals, comparator);
        int last = Integer.MIN_VALUE;
        for(Interval ite : intervals){
            if(last < ite.start){
                list.add(ite);
                last = ite.end;
            }else{
                last = Math.max(last, ite.end);
                list.get(list.size()-1).end = last;
            }
        }
        return list;
    }
    
    
}
```

