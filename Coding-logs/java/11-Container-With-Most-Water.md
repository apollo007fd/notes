# 11. Container With Most Water
#### 给一个数组，求形成的容器的最大装水量，两条线和x轴形成一个容器
#### 避免全局搜索，只搜索那些可能包含最大解的情况
#### 左右边界，往中间移动，每次移动值较小的那个边界


    class Solution {
        public int maxArea(int[] height) {
            int i=0, j=height.length-1;
            int minH = Integer.MAX_VALUE;
            int maxV = 0;
            while(i<=j) {
                // find the lowest one
                maxV = Math.max(maxV, Math.min(height[i], height[j]) * (j-i));
                if (height[i] > height[j])
                    j--;
                else
                    i++;
            }
            return maxV;
        }
    }

