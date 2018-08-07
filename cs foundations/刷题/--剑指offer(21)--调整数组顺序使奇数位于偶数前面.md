# 调整数组顺序使奇数位于偶数前面

输入一个整数数组, 实现一个函数来调整该数组中数字的顺序,使得所有奇数位于数组的前半部分, 所有偶数位于数组的后半部分.



快速排序的思想, 从前往后, 找一个偶数, 从后往前找一个奇数, 交换之, 知道前后相遇.

```
public static void ReOrderOddEven(int[] array){
    if(array==null || array.length==0 || array.length==1)
    	return ;
    int head=0, tail=array.length-1;
    while(head < tail){
        while(head <tail && judge(array[head])) head++;
        while(tail > head && !judge(array[tail])) tail--;
        swap(array, head, tail);
    }
}

//将判断是否为奇数的函数写在这里, 如果功能需要扩展, 比如要将功能变为:使负数位于正数前面, 只需修改这个函数即可.
public static boolean judge(int n){
    return (n & 0x1 == 1);
}

public static void swap(int[] array, int i, int j){
    int tmp = array[i];
    array[i] = array[j];
    array[j] = tmp;
}
```

**注意:**

代码判断最好解耦....
