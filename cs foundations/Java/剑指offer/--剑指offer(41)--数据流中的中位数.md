## 题目描述
如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

## 解法一 最大堆、最小堆法  
维护两个堆：  
1.保持最小堆的根结点大于最大堆的所有节点  
2.保持两个堆中的结点数目最多只相差1  

代码如下：  

    import java.util.Comparator;
    import java.util.PriorityQueue;
    public class Solution {
        private int count = 0;
        private PriorityQueue<Integer> min = new PriorityQueue<Integer>();
        private PriorityQueue<Integer> max = new PriorityQueue<Integer>(15, new Comparator<Integer>(){
            public int compare(Integer i1, Integer i2){
              return i2 - i1;
            }
        });
        public void Insert(Integer num) {
          if(count % 2 == 0){
            max.add(num);
            int topMax = max.poll();
            min.add(topMax);
          }else{
            min.add(num);
            int topMin = min.poll();
            max.add(topMin);
          }
          count++;
        }
        public Double GetMedian() {
            if(count % 2 == 0){
              return new Double(max.peek() + min.peek()) / 2;
            }else
              return new Double(min.peek());
        }
    }
