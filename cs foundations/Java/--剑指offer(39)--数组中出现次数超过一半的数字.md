## [题目]
数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字，如果没有满足要求的数字，输出0。
例如，输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2出现在数组中5次，超过数组长度的一半，因此输出2.

## [思路1]
基于字典，key存放数组中元素，value存放出现次数；再找出value最大的key.  时间复杂度o(n) 、空间复杂度o(n)

    import java.util.HashMap;
    import java.util.Set;
    public static int most_element_dict(int[] array){
      HashMap<Integer, Integer> dict = new HashMap<Integer, Integer>();
      for(int i: array){
        if(dict.containsKey(i)){
          dict.put(i) = dict.get(i)+1;
        }else{
          dict.put(i) = 1;
        }
      }
      Set<Integer> set = dict.keySet();
      for(Integer s: set){
        if(dict.get(s) > array.length / 2)
          return s;
      }
      return 0;
    }
   
## [思路2]
基于Partition函数的时间复杂度为o(n)的算法，出现大于数组长度一半的数字，
假设存在满足要求的数字，那么将数组由小到大排序，中位数一定是出现次数最多的元素。
排序的最小时间复杂度为o(nlogn),但是求中位数有o(n)的算法；
随机选取一个元素，遍历数组，将小于它的放左边，大于它的放右边，
如果它是中位数，那么它就是res，
如果它在数组最中间元素左边，那么res在它的右边，去它的右边部分查找；
如果它在数组最中间元素右边，那么res在它的左边，去它的左边部分查找；
是递归过程；

    public static int most_element_partition(int[] array, int i, int j){
      if(array.length==0)
        return 0;
      int start = 0;
      int end = array.length-1;
      int length = array.length;
      int threshold = partition(array, 0, array.length-1);
      int middle = length >> 1;  //位运算比除法高效
      while(threshold != middle){
        if(threshold > middle){
          threshold = partition(array, start, threshold);
        }else{
          threshold = partition(array, threshold+1, end);
        }
      }
      int result = array[middle];
      int times = 0;
      for(int i: array){
        if(i==result)
          times++;
      }
      if(times *2 > array.length){
        return result;
      }
      return 0;
    }
    public static int partition(int[] array, int from, int to){
      if(from > to)
        return -1;
      while(from < to){
        while(array[to] >= array[from] && from < to) to--;
        swap(array, from, to);
        while(array[from] < array[to] && from < to) from++;
        swap(array, to, from);
      }
      return from;
    }
    public static void swap(int[] array, int i, int j){
      int tmp = array[i];
      array[i] = array[j];
      array[j] = tmp;
    }

## [思路3]
就叫他为“同归于尽法”吧，
假设数组中有一个数字出现的次数超过数组长度的一半，那么，可以在遍历数组的时候保存2个值，一个是数组中的一个数字，另一个是数字出现的次数；
当我们遍历到下一个数字的时候，如果下一个数字和我们之前保存的数字相同，则次数加1；
如果下一个数字和我们之前保存的数字不同，则次数减1；
如果次数为零，那么保存下一个遍历的数字，并把次数记为1；
如果存在满足题目要求的数字(出现次数多于数组长度一半,最后需要判断)，最后的数字，就是我们要找的;

public static int most_element_eliminate(int[] array){
  int num = array[0];
  int times = 1;
  for(int i=1; i<array.length; i++){
    if(array[i]!=num){
      if(times==0){
        num = array[i];
        times = 1;
      }else{
        times--;
      }
    }else{
      times++;
    }
  }
  if(times==0)
    return 0;
  int count = 0;
  for(int i:array)
    if(i==num)
      count++;
  if(2*count>=array.length)
    return num;
  return 0;
}

参考：剑指offer第2版
