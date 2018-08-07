729. My Calendar I 



安排时间计划, 给出若干个记录, 每个记录给出初始时间start, 结束时间end, 判断是否所有的时间计划都不重叠.

时间范围[0,10^9], 测试用例在1000个以内.

不能用数组模拟,因为时间范围太大.



蛮力法:

当没加入一个时间区间时, 跟之间已经加入的逐条进行比较, 如果没有重叠就加入并返回true, 否则返回false.



完美解法:

使用TreeMap存储start和end, 以start为key, end为value.

TreeMap中的key默认按从小到大排列.

```java
class MyCalendar {
    
    // HashMap<start, end>
    // 
    TreeMap<Integer, Integer> booked;
    public MyCalendar() {
        booked = new TreeMap<Integer, Integer>();
    }
    
    public boolean book(int start, int end) {
        Integer f1 = booked.floorKey(start);
        if(f1!=null && booked.get(f1) > start) return false;
        Integer f2 = booked.ceilingKey(start);
        if(f2!=null && f2 < end) return false;
        booked.put(start, end);
        return true;
    }
}

/**
 * Your MyCalendar object will be instantiated and called as such:
 * MyCalendar obj = new MyCalendar();
 * boolean param_1 = obj.book(start,end);
 */
```



