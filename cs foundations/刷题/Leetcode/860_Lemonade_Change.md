860. Lemonade Change 



购物者排队,用5,10,20元购买物品,判断能不能为所有人找零钱成功.



尽量先用大的10元的找零, 10元不够再用5元的找零, 零钱不够返回false.

```java
class Solution {
    public boolean lemonadeChange(int[] bills) {
        int n5 = 0, n10=0;
        for(int n:bills){
            if(n==5)
                n5++;
            else if(n==10){
                if(n5!=0){
                    n5--;
                    n10++;
                }else
                    return false;
            }else{
                if(n10!=0 && n5!=0){
                    n10--;
                    n5--;
                }else if(n5>=3){
                    n5-=3;
                }else
                    return false;
            }
        }
        return true;
    }
}
```

