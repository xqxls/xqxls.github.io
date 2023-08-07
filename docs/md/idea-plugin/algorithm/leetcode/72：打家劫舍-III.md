## 题目描述 
>  小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为 `root` 。
>
>  除了 `root` 之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果 **两个直接相连的房子在同一天晚上被打劫** ，房屋将自动报警。
>
>  给定二叉树的 `root` 。返回 在不触动警报的情况下 ，小偷能够盗取的最高金额 。
>
>   
>
>  **示例 1:**
>
>  ![img](https://assets.leetcode.com/uploads/2021/03/10/rob1-tree.jpg)
>
>  ```
>  输入: root = [3,2,3,null,3,null,1]
>  输出: 7 
>  解释: 小偷一晚能够盗取的最高金额 3 + 3 + 1 = 7
>  ```
>
>  **示例 2:**
>
>  ![img](https://assets.leetcode.com/uploads/2021/03/10/rob2-tree.jpg)
>
>  ```
>  输入: root = [3,4,5,1,3,null,1]
>  输出: 9
>  解释: 小偷一晚能够盗取的最高金额 4 + 5 = 9
>  ```


## 方法一（树状dp）
#### 1.解题思路
对于每一个节点，分为选择和不选择两种情况。通过递归，我们可以很方便地获得当前左右子树的对应两种情况，然后根据左右子树的情况计算当前节点的情况。如果选择当前节点，则左右子节点不能选择，如果不选择当前节点，则可以选也可以不选左右子节点，取较大者。

#### 2.代码实现
```java
class Solution {
    public int rob(TreeNode root) {
        return Math.max(dfs(root)[0],dfs(root)[1]);
    }

    private int[] dfs(TreeNode root){
        if(root==null){
            return new int[2];
        }
        int[] l = dfs(root.left);
        int[] r = dfs(root.right);
        int select = root.val+l[1]+r[1];
        int notSelect = Math.max(l[0],l[1])+Math.max(r[0],r[1]);
        return new int[]{select,notSelect};
    }
}
```
#### 3.复杂度分析

- 时间复杂度：需要遍历树中所有节点，所以时间复杂度为O(n)。
- 空间复杂度：需要额外大小为n的dp数组，所以空间复杂度为O(n) 。

