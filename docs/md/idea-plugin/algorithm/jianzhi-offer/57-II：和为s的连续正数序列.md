# 题目描述
> 输入一个正整数 target ，输出所有和为 target 的连续正整数序列（至少含有两个数）。 序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。
> 示例 1：
> 输入：target = 9 
> 输出：[[2,3,4],[4,5]] 
> 示例 2：
> 输入：target = 15 
> 输出：[[1,2,3,4,5],[4,5,6],[7,8]]  
> 限制：
> 1 <= target <= 10^5
## 方法一（滑动窗口）
#### 1.解题思路
维护一个头是start，尾是end的滑动窗口，起初start为1，end为2，不断计算滑动窗口内所有数的和sum，当sum等于target时，找到对应的序列，加入结果集，并且滑动窗口头部后移一位，继续循环；如果大于target，直接头部后移；如果小于target，则尾部后移。
#### 2.代码实现

```java
class Solution {
    public int[][] findContinuousSequence(int target) {
        ArrayList<int[]> list=new ArrayList<>();
        int start=1;
        int end=2;
        while(start<end){
            int sum=(start+end)*(end-start+1)/2;
            if(sum==target){
                list.add(generate(start,end));
                start++;               
            }
            else if(sum>target){
                start++;   
            }
            else{
                end++;
            }
        }
        return list.toArray(new int[list.size()][]);
        
    }
    private int[] generate(int start,int end){
        int[] res=new int[end-start+1];
        for(int i=start;i<=end;i++){
            res[i-start]=i;
        }
        return res;
    }
}
```
#### 3.复杂度分析
 - 时间复杂度：start最大为(target-1)/2，循环最多执行(target-1)/2次，所以时间复杂度为O(target)。
 - 空间复杂度：需要额外常数级别的内存空间，所以空间复杂度为O(1)。
