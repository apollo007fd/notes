# java集合框架使用总结

- 队列
- 栈
- List
- map



# 队列

Queue是队列接口, LinkedList类实现了Queue接口, [LinkedList的API](https://docs.oracle.com/javase/7/docs/api/java/util/LinkedList.html)如下:

| 返回值  |  API定义  |           功能           |
| :-----: | :-------: | :----------------------: |
| boolean | add(E e)  |   元素e从尾部加入队列    |
|    E    |  poll()   | 队列头部元素出队列并返回 |
| boolean | isEmpty() |     判断队列是否为空     |



# List

双层List的定义:

```
List<List<Integer>> result = new ArrayList<List<Integer>>();
```

错误: 指定2个ArrayList

```
List<List<Integer>>result = new ArrayList<ArrayList<Integer>>();
```


# Map

高赞贴: https://www.cnblogs.com/chenssy/p/3264214.html

- HashMap: 我们最常用的Map, 它根据key的HashCode值来存储数据, 根据key可以直接获取它的value, 同时它具有很快的访问速度. HashMap最多只允许一条记录的key值为null(多条会覆盖); 允许多条记录的Value为null. 非同步的.
- TreeMap: 能够把它保存的记录根据key排序, 默认是按升序排序, 也可以指定排序的比较器, 当用Iterator遍历TreeMap时, 得到的记录是排过序的. TreeMap不允许key的值为null. 非同步的.
 - Hashtable: 与HashMap类似, 不同的是: key和value的值均不允许为null; 它支持线程的同步, 即任一时刻只有一个线程能写Hashtable, 因此也导致了Hashtable在写入时会比较慢.
 - LinkedHashMap: 保存了记录的插入顺序, 在用Iterator遍历LinkedHashMap时, 先得到的记录肯定是先插入的. 在遍历的时候会比HashMap慢. key和value均允许为空, 非同步的.

## TreeMap

按key排序的Map, 可以用Comparator接口指定key按升序还是降序排列.

常用api

| 返回值 | API                | function                                                     |
| ------ | ------------------ | ------------------------------------------------------------ |
| V      | put(key, value)    |                                                              |
| V      | get(key)           |                                                              |
| Set<K> | keySet()           |                                                              |
| K      | floorKey(K key)    | 返回key之前(包括自己)最大的键, 如果没有满足条件的返回null    |
| K      | ceilingKey(K key)  | 返回key之后(包括自己)最小的键, 如果没有满足条件的返回null    |
| V      | remove(Object key) | Removes the mapping for this key from this TreeMap if present. |

```java
public class TreeMapTest {
    public static void main(String[] args) {
        Map<String, String> map = new TreeMap<String, String>(
                new Comparator<String>() {
                    public int compare(String obj1, String obj2) {
                        // 降序排序
                        return obj2.compareTo(obj1);
                    }
                });
        map.put("c", "ccccc");
        map.put("a", "aaaaa");
        map.put("b", "bbbbb");
        map.put("d", "ddddd");
        
        Set<String> keySet = map.keySet();
        Iterator<String> iter = keySet.iterator();
        while (iter.hasNext()) {
            String key = iter.next();
            System.out.println(key + ":" + map.get(key));
        }
    }
}

运行结果如下：
d:ddddd 
c:ccccc 
b:bbbbb 
a:aaaaa
```



### TreeMap按value排序

```java
public static void main(String[] args) {
		TreeMap<String, Integer> map = new TreeMap<String, Integer>();
		map.put("acb1", 5);
		map.put("bac1", 3);
		map.put("bca1", 20);
		map.put("cab1", 80);
	    map.put("cba1", 1);
	    map.put("abc1", 10);
	    map.put("abc2", 12);
	    
	    //Map.Entry<String, Integer>得到一个HashMap的记录的数据类型.
	    Comparator<Map.Entry<String, Integer>> valueComparator = new Comparator<Map.Entry<String, Integer>>(){
			@Override
			public int compare(Entry<String, Integer> o1,
					Entry<String, Integer> o2) {
				return o1.getValue() - o2.getValue();
			}
	    };
	    
	    List<Map.Entry<String, Integer>> list = new ArrayList<Map.Entry<String, Integer>>(map.entrySet()); 
	    Collections.sort(list, valueComparator); // Collections.sort()进行排序
	    for(Map.Entry<String, Integer> entry:list){
	    	System.out.println(entry.getKey() + ":"+entry.getValue());
	    }
	}
```

几种用法:

定义一个HashMap键值对记录的泛型类: Map.Entry<String, Integer>

相关试题: [网易-会话列表](https://www.nowcoder.com/question/next?pid=11647029&qid=117506&tid=17193456) 