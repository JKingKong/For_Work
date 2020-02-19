---

---

<!-- GFM-TOC -->

* [斐波那契数列](#斐波那契数列)
    * [1. 爬楼梯](#1-爬楼梯)
    * [2. 强盗抢劫](#2-强盗抢劫)
    * [3. 强盗在环形街区抢劫](#3-强盗在环形街区抢劫)
    * [4. 信件错排](#4-信件错排)
    * [5. 母牛生产](#5-母牛生产)
* [矩阵路径](#矩阵路径)
    * [1. 矩阵的最小路径和](#1-矩阵的最小路径和)
    * [2. 矩阵的总路径数](#2-矩阵的总路径数)
* [数组区间](#数组区间)
    * [1. 数组区间和](#1-数组区间和)
    * [2. 数组中等差递增子区间的个数](#2-数组中等差递增子区间的个数)
* [分割整数](#分割整数)
    * [1. 分割整数的最大乘积](#1-分割整数的最大乘积)
    * [2. 按平方数来分割整数](#2-按平方数来分割整数)
    * [3. 分割整数构成字母字符串](#3-分割整数构成字母字符串)
* [最长递增子序列](#最长递增子序列)
    * [1. 最长递增子序列](#1-最长递增子序列)
    * [2. 一组整数对能够构成的最长链](#2-一组整数对能够构成的最长链)
    * [3. 最长摆动子序列](#3-最长摆动子序列)
* [最长公共子序列](#最长公共子序列)
    * [1. 最长公共子序列](#1-最长公共子序列)
* [0-1 背包](#0-1-背包)
    * [1. 划分数组为和相等的两部分](#1-划分数组为和相等的两部分)
    * [2. 改变一组数的正负号使得它们的和为一给定数](#2-改变一组数的正负号使得它们的和为一给定数)
    * [3. 01 字符构成最多的字符串](#3-01-字符构成最多的字符串)
    * [4. 找零钱的最少硬币数](#4-找零钱的最少硬币数)
    * [5. 找零钱的硬币数组合](#5-找零钱的硬币数组合)
    * [6. 字符串按单词列表分割](#6-字符串按单词列表分割)
    * [7. 组合总和](#7-组合总和)
* [股票交易](#股票交易)
    * [1. 需要冷却期的股票交易](#1-需要冷却期的股票交易)
    * [2. 需要交易费用的股票交易](#2-需要交易费用的股票交易)
    * [3. 只能进行两次的股票交易](#3-只能进行两次的股票交易)
    * [4. 只能进行 k 次的股票交易](#4-只能进行-k-次的股票交易)
* [字符串编辑](#字符串编辑)
    * [1. 删除两个字符串的字符使它们相等](#1-删除两个字符串的字符使它们相等)
    * [2. 编辑距离](#2-编辑距离)
    * [3. 复制粘贴字符](#3-复制粘贴字符)
<!-- GFM-TOC -->




递归和动态规划都是将原问题拆成多个子问题然后求解，他们之间最本质的区别是，动态规划保存了子问题的解，避免重复计算。

# 斐波那契数列

## 1. 爬楼梯

70\. Climbing Stairs (Easy)

[Leetcode](https://leetcode.com/problems/climbing-stairs/description/) / [力扣](https://leetcode-cn.com/problems/climbing-stairs/description/)

题目描述：有 N 阶楼梯，每次可以上一阶或者两阶，求有多少种上楼梯的方法。

定义一个数组 dp 存储上楼梯的方法数（为了方便讨论，数组下标从 1 开始），dp[i] 表示走到第 i 个楼梯的方法数目。

第 i 个楼梯可以从第 i-1 和 i-2 个楼梯再走一步到达，走到第 i 个楼梯的方法数为走到第 i-1 和第 i-2 个楼梯的方法数之和。

<!--<div align="center"><img src="https://latex.codecogs.com/gif.latex?dp[i]=dp[i-1]+dp[i-2]" class="mathjax-pic"/></div> <br>-->

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/14fe1e71-8518-458f-a220-116003061a83.png" width="200px"> </div><br>
考虑到 dp[i] 只与 dp[i - 1] 和 dp[i - 2] 有关，因此可以只用两个变量来存储 dp[i - 1] 和 dp[i - 2]，使得原来的 O(N) 空间复杂度优化为 O(1) 复杂度。

```java
class Solution {
    public int climbStairs(int n) {
        if(n < 0) return 0;
        if(n <= 2) return n;
        int pre1=1;int pre2=2;
        int sum = 0;
        for(int i=3;i <= n;i++){
            sum = pre1 + pre2;
            pre1 = pre2;
            pre2 = sum;
        }
        return sum;
    }   
}
```

### 斐波那契数列矩阵快速幂，并且使用位运算加速

```java
public class Solution {
   // 斐波那契数列矩阵快速幂，并且使用位运算加速
   public int climbStairs(int n) {
       int[][] q = {{1, 1}, {1, 0}};
       int[][] res = pow(q, n);
       return res[0][0];
   }
   public int[][] pow(int[][] a, int n) {
       int[][] ret = {{1, 0}, {0, 1}}; //初始为单位矩阵
       // 快速幂，并且使用位运算加速
       while (n > 0) {
           if ((n & 1) == 1) {
               //(n & 1) == 1判断n为奇数时,之前的结果ret 与 a 做矩阵乘法    ret = ret * a * (a * a)
               ret = multiply(ret, a); // ret = ret * a
           }
           n = n >> 1; // 即：n/2
           a = multiply(a, a); //基数扩大a^2
       }
       return ret;
   }
   public int[][] multiply(int[][] a, int[][] b) {
       // 矩阵乘法
       int[][] c = new int[2][2];
       for (int i = 0; i < 2; i++) {
           for (int j = 0; j < 2; j++) {
               c[i][j] = a[i][0] * b[0][j] + a[i][1] * b[1][j];
           }
       }
       return c;
   }
}
```

## 2. 强盗抢劫

198\. House Robber (Easy)

[Leetcode](https://leetcode.com/problems/house-robber/description/) / [力扣](https://leetcode-cn.com/problems/house-robber/description/)

题目描述：抢劫一排住户，但是不能抢邻近的住户，求最大抢劫量。

**动态规划**

*   动态规划方程：`dp[n] = MAX( dp[n-1], dp[n-2] + num )`
*   由于不可以在相邻的房屋闯入，所以在当前位置 `n` 房屋可盗窃的最大值，要么就是 `n-1` 房屋可盗窃的最大值，要么就是 `n-2` 房屋可盗窃的最大值加上当前房屋的值，二者之间取最大值
*   举例来说：1 号房间可盗窃最大值为 3即为 `dp[1]=3`，2 号房间可盗窃最大值为 4即为 `dp[2]=4`，3 号房间自身的值为 即为 `num=2`，那么 `dp[3] = MAX( dp[2], dp[1] + num ) = MAX(4, 3+2) = 5`，3 号房间可盗窃最大值为5
*   `dp[i]` 代表前  `i` 个房子在满足条件下的能偷窃到的最高金额。
*   时间复杂度：`O(n)` 
*   空间复杂度：`o(n)`

```java
class Solution {
    public int rob(int[] nums) {
        // 
        if(nums == null || nums.length == 0) return 0;
        if(nums.length == 1) return nums[0];
        if(nums.length == 2) return Math.max(nums[0],nums[1]);
        int[] dp = new int[nums.length + 1]; //抢劫n间房屋
        dp[0] = 0;
        dp[1] = nums[0];
        for(int i = 2;i <= nums.length;i++){
            // dp[i] ：i间屋子的最大金额
            // dp[i-2] + nums[i-1]：i-2间屋子的最大金额 + 第i间屋子的最大金额(nums[i-1])
            // dp[i-1]： 不抢第i间屋子的最大金额，也就是说抢了前i-1间
            dp[i] = Math.max(dp[i-2] + nums[i-1], dp[i-1]);
        }
        return dp[nums.length]; //抢劫n间房屋的最大金额
    }
}
```

由于**dp[i]** 只用到前两个**dp[i-2]**和**dp[i-1]** 所以只需要两个变量来保存就好了

```java
class Solution {
    public int rob(int[] nums) {
        // pre2 类似dp[i-2]  pre1类似dp[i-1]
        int pre2 = 0,pre1=0;
        for(int num : nums){
            int cur = Math.max(pre1, pre2 + num);
            pre2 = pre1;
            pre1 = cur;
        }
        return pre1;
    }
}
```



## 3. 强盗在环形街区抢劫

213\. House Robber II (Medium)

[Leetcode](https://leetcode.com/problems/house-robber-ii/description/) / [力扣](https://leetcode-cn.com/problems/house-robber-ii/description/)

### **解题思路**

* 动态规划
* 状态转移方程`max(rob(nums, 0, n - 2), rob(nums, 1, n - 1))`
* **环状排列**意味着**第一个房子**和**最后一个房子**中**只能选择一个偷窃**（这样问题就转换成了，打家劫舍I的类型）：
  * 在不偷窃第一个房子的情况下----`p1 = rob(nums，0，n-2);`
  * 在不偷窃最后一个房子的情况下---=`p2 = rob(nums，1，n-1)；`
    * **综合偷窃最大金额：** 为以上两种情况的较大值，即 `max(p1,p2)`

```java
class Solution {
    public int rob(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        int n = nums.length;
        if (n == 1) {
            return nums[0];
        }
        return Math.max(rob(nums, 0, n - 2), rob(nums, 1, n - 1));
    }

    private int rob(int[] nums, int first, int last) {
        // 这里的处理类似打家劫舍I，但是是按照区间范围的
        int pre2 = 0, pre1 = 0;
        for (int i = first; i <= last; i++) {
            int cur = Math.max(pre1, pre2 + nums[i]);
            pre2 = pre1;
            pre1 = cur;
        }
        return pre1;
    }
}
```

## 4. 信件错排

题目描述：有 N 个 信 和 信封，它们被打乱，求错误装信方式的数量。

定义一个数组 dp 存储错误方式数量，dp[i] 表示前 i 个信和信封的错误方式数量。假设第 i 个信装到第 j 个信封里面，而第 j 个信装到第 k 个信封里面。根据 i 和 k 是否相等，有两种情况：

- i==k，交换 i 和 j 的信后，它们的信和信封在正确的位置，但是其余 i-2 封信有 dp[i-2] 种错误装信的方式。由于 j 有 i-1 种取值，因此共有 (i-1)\*dp[i-2] 种错误装信方式。
- i != k，交换 i 和 j 的信后，第 i 个信和信封在正确的位置，其余 i-1 封信有 dp[i-1] 种错误装信方式。由于 j 有 i-1 种取值，因此共有 (i-1)\*dp[i-1] 种错误装信方式。

综上所述，错误装信数量方式数量为：

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/da1f96b9-fd4d-44ca-8925-fb14c5733388.png" width="350px"> </div><br>
## 5. 母牛生产

[程序员代码面试指南-P181](#)

题目描述：假设农场中成熟的母牛每年都会生 1 头小母牛，并且永远不会死。第一年有 1 只小母牛，从第二年开始，母牛开始生小母牛。每只小母牛 3 年之后成熟又可以生小母牛。给定整数 N，求 N 年后牛的数量。

第 i 年成熟的牛的数量为：

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/879814ee-48b5-4bcb-86f5-dcc400cb81ad.png" width="250px"> </div><br>
# 矩阵路径

## 1. 矩阵的最小路径和

64\. Minimum Path Sum (Medium)

[Leetcode](https://leetcode.com/problems/minimum-path-sum/description/) / [力扣](https://leetcode-cn.com/problems/minimum-path-sum/description/)

```html
[[1,3,1],
 [1,5,1],
 [4,2,1]]
Given the above grid map, return 7. Because the path 1→3→1→1→1 minimizes the sum.
```

题目描述：求从矩阵的左上角到右下角的最小路径和，每次只能向右和向下移动。\

* 思路
  1. 暴力递归--超时
  2. 二维动态规划 ---- 新建一个二维dp数组
  3. 一维动态规划 ---- 源数组作为dp数组

```java
class Solution {
    public int minPathSum(int[][] grid) {
        if(grid == null || grid.length == 0) return 0;
        //算法从 左上--->右下
        // 源数组作为动态规划的数组,也可以新建一个二维dp数组
        for(int i=0;i<grid.length;i++){
            for(int j=0;j<grid[0].length;j++){
                if(i == 0 && j == 0){
                    //起始点
                     continue;
                }else if(i == 0){
                    //当前是上边界时，只能从左边来
                    grid[i][j] = grid[i][j-1] + grid[i][j];
                }else if(j == 0){
                    //当前是左边界时，只能从上边来
                    grid[i][j] = grid[i-1][j] + grid[i][j];
                }else{
                    // 不在上边界，也不在下边界
                    grid[i][j] = grid[i][j] + Math.min(grid[i][j-1],grid[i-1][j]);
                }
            }
        }
        return grid[grid.length - 1][grid[0].length - 1];
    }
}
```

递归暴力法--超时

```java
class Solution {
    public int minPathSum(int[][] grid) {
        if(grid == null) return 0;

        return (int)minFee(grid,0,0);
    }
    // 递归法暴力法超时
    private long minFee(int[][] grid,int i,int j){
        if(i >= grid.length || j >= grid[0].length)
            return Integer.MAX_VALUE; //走超出网格的路,则返回一个很大的代价
        if(i == grid.length - 1 && j == grid[0].length - 1)
            return grid[i]][j];//到出口
        long minfee = grid[i][j] + Math.min(minFee(grid,i,j+1),  //向右走
                                             minFee(grid,i+1 ,j) // 向下走
                                            );
        return minfee;
    }
}
```







## 2. 矩阵的总路径数

62\. Unique Paths (Medium)

[Leetcode](https://leetcode.com/problems/unique-paths/description/) / [力扣](https://leetcode-cn.com/problems/unique-paths/description/)

题目描述：统计从矩阵左上角到右下角的路径总数，每次只能向右或者向下移动。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/dc82f0f3-c1d4-4ac8-90ac-d5b32a9bd75a.jpg" width=""> </div><br>

### 方法一：二维动态规划

```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];
        // 从左上角--->右下角
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(i == 0 && j== 0){
                    //在起点时
                    dp[i][j] = 1;
                }else if(i == 0){
                    // 在上边界时，只能从左边过来，所以上边界只有一种走法
                    dp[i][j] = 1;
                }else if(j == 0){
                    // 在左边界时，只能从上边过来，所以上边界只有一种走法
                    dp[i][j] = 1;
                }else{
                    // 既不在上边界也不再左边界，则可以从左边 或者 上边过来
                    // dp[i][j-1]从左边走的路径数
                    // dp[i-1][j]从上边走的路径数
                    dp[i][j] =  dp[i][j-1] + dp[i-1][j]; 
                }
            }
        }
        return dp[m-1][n-1];
    }
}
```

### 方法二：一维动态规划（空间压缩，要在一的基础上进行分析）

```java
public int uniquePaths(int m, int n) {
    int[] dp = new int[n];
    Arrays.fill(dp, 1); 
    for (int i = 1; i < m; i++) {
        // 从1开始
        for (int j = 1; j < n; j++) {
            // 从1开始
            dp[j] = dp[j] + dp[j - 1];
        }
    }
    return dp[n - 1];
}
```

### 方法三：数学公式法

也可以**直接用数学公式求解**，这是一个**组合问题**。机器人总共移动的次数 S=m+n-2，向下移动的次数 D=m-1，那么问题可以看成从 S 中取出 D 个位置的组合数量，这个问题的解为 C(S, D)。

```java
class Solution {
    public int uniquePaths(int m, int n) {
        int S = m + n - 2;  // 总共的移动次数
        int D = m - 1;      // 向下的移动次数
        long ret = 1;
        // 排列组合公式转换成阶乘计算
        for (int i = 1; i <= D; i++) {
            ret = ret * (S - D + i) / i;
        }
        return (int) ret;
    }
}
```

# 数组区间

## 1. 数组区间和

303\. Range Sum Query - Immutable (Easy)

[Leetcode](https://leetcode.com/problems/range-sum-query-immutable/description/) / [力扣](https://leetcode-cn.com/problems/range-sum-query-immutable/description/)

```html
Given nums = [-2, 0, 3, -5, 2, -1]

sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3
```

### 暴力法

```java
class NumArray {
    private int[] nums;
    public NumArray(int[] nums) {
        this.nums = nums;
    }
    
    public int sumRange(int i, int j) {
        if(i < 0 || j >= nums.length) return 0;
        int sum = 0;
        while(i <= j){
            sum += nums[i];
            i++;
        }
        return sum;
    }
}

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * int param_1 = obj.sumRange(i,j);
 */
```

### 动态规划

求区间 i \~ j 的和，可以转换为 sum[j + 1] - sum[i]，其中 sum[i] 为 0 \~ i - 1 的和。

```java
class NumArray {
    private int[] nums;
    private int[] sum;
    public NumArray(int[] nums) {
        this.nums = nums;
        if(nums != null){
            int[] sum = new int[nums.length + 1];
            //sum[i---j] = sum[0--j] - sum[0--i]
            // sum[index]: nums数组，0---index的和
            for(int i=1;i <= nums.length;i++){
                sum[i] = nums[i-1] + sum[i-1];
            }
            this.sum = sum;
        }
    }
    public int sumRange(int i, int j) {
        if(nums == null) return 0;
        if(i < 0 || j >= nums.length) return 0;
        //sum[i---j] = sum[0--j] - sum[0--i]
        return sum[j+1] - sum[i];
    }
}
```

## 2. 数组中等差递增子区间的个数

413\. Arithmetic Slices (Medium)

[Leetcode](https://leetcode.com/problems/arithmetic-slices/description/) / [力扣](https://leetcode-cn.com/problems/arithmetic-slices/description/)

思路：

如果区间 `(i, j)`是等差数列，那么当 `A[j+1]`和 `A[j]`的差值和之前的**差值相等**的情况下，区间 `(i,j+1)` 也构成一个等差数列。此外，如果区间 `(i,j)`就不是一个等差数列，那么之后再向右拓展也不可能是一个等差数列了

* 核心思想：`(0,i-1)`是等差数列且其子等差数列数量为：`x个`，而加上第 `i`个数字时，也成为等差数列 `(0,i)`，则子等差数列的增量为：`x+1`，理由如下：

  * 其中新增等差数列的区间为 `(0,i), (1,i), ... (i-2,i)`，这些区间总数为 `x+1`

  * 而除了区间 `(0,i)` 以外，其余的区间如（可以采用整体向左平移一位理解）

     `(1,i), (2,i),...(i-2,i)` 这些都可以对应到之前的区间 

    `(0,i-1), (1,i-1),...(i-3,i-1)` 上去，其值为 `x`

* 状态转移方程： `dp[i] = dp[i-1] + 1`     （即：加上 nums[i] 后还为等差数列，则增加的等差子序列的数量）

* 等差子数列数量总和：`sum += dp[i]`

### 方法一：动态规划，使用dp数组

```java
public class Solution {
    public int numberOfArithmeticSlices(int[] A) {
        int[] dp = new int[A.length];
        int sum = 0;
        for (int i = 2; i < dp.length; i++) {
            if (A[i] - A[i - 1] == A[i - 1] - A[i - 2]) {
                dp[i] = 1 + dp[i - 1];
                sum += dp[i];
            }
        }
        return sum;
    }
}
```

### 方法二：动态规划，常量空间

**由方法一观察后得到的优化**

在上一个方法中，可以观察到我们其实只需要 `dp[i-1]` 来决定 `dp[i]`的值。因此，相对于整个 `dp`数组，我们只需要保存一个最近一个 `dp` 值就可以了。

```java
public class Solution {
    public int numberOfArithmeticSlices(int[] A) {
        int dp = 0;
        int sum = 0;
        for (int i = 2; i < A.length; i++) {
            if (A[i] - A[i - 1] == A[i - 1] - A[i - 2]) {
                dp = 1 + dp;
                sum += dp;
            } else{
                dp = 0; //表示之前的等差数列的子数组个数为0
            }
        }
        return sum;
    }
}
```



# 分割整数

## 1. 分割整数的最大乘积

343\. Integer Break (Medim)

[Leetcode](https://leetcode.com/problems/integer-break/description/) / [力扣](https://leetcode-cn.com/problems/integer-break/description/)

题目描述：For example, given n = 2, return 1 (2 = 1 + 1); given n = 10, return 36 (10 = 3 + 3 + 4).

### 方法一：（观察法 + 动态规划）

**解题思路**：

* `尽可能有3和2`

* `dp[i]`为整数`i`拆分后的最大乘积

* | 2=1+1              |                                1 * 1 = 1 |
  | ------------------ | ---------------------------------------: |
  | 3=1+2              |                                1 * 2 = 3 |
  | 4=2+2              |                                2 * 2 = 4 |
  | 5=3+2              |                                3 * 2 = 6 |
  | 6=3+3              |                                3 * 3 = 9 |
  | 7=3+2+2            |      3 * 2 * 2 = 3 * dp[7-3] = 3 * dp[4] |
  | 8=3+3+2            |     3 * 3 * 2  = 3 * dp[8-3] = 3 * dp[5] |
  | 9=3+3+3 = 3 + 6    |      3 * 3 * 3 = 3 * dp[9-3] = 3 * dp[6] |
  | 10=3+3+2+2 = 3 + 7 | 3 * 3 * 2 * 2 = 3 * dp[10-3] = 3 * dp[7] |

* `转移方程：n = 3 * dp[n-3]   且n-3 > 3，即：n > 6时成立`

* 由于只用到了`dp[n-3]`，因此只需要3个变量保存，`dp[n-3],dp[n-2],dp[n-1]`，这样就能不断递推下去

```java
class Solution {
    public int integerBreak(int n) {
        //n <= 6时的情况
        if(n == 2) return 1;
        if(n == 3) return 2;
        if(n == 4) return 4;
        if(n == 5) return 6;
        if(n == 6) return 9;
        
        // n > 6时的情况
        int p3=4,p2=6,p1=9;
        int res = 0;
        for(int i=7;i<=n;i++){
            res = 3 * p3;
            p3 = p2;
            p2 = p1;
            p1 = res;
        }
        return res;
    }
}
```

### 方法二：公式法

* 尽可能的让3多一些

* 根据 `余数 = n % 3`的情况进行操作

```java
class Solution {
    public int integerBreak(int n) {   
        if(n <= 3) return n - 1;   //2,3时
        int count3 = n / 3;
        int m = n % 3;
        if(m == 0){
            return (int)Math.pow(3,count3);
        }else if(m == 1){
            // 10=3+3+3+1          3*3*3*1=9
            // 10=3+3+(3+1)=3+3+4  3*3*4=36
            return (int)Math.pow(3,count3 - 1) * 4;
        }   
        //当m == 2时
        // 8=3+3+2  3*3*2=18
        return (int)Math.pow(3,count3) * 2;  
    }
}
```

### 方法三 暴力递归--->记忆化搜索--->动态规划

[LeetCode题解](https://leetcode-cn.com/problems/integer-break/solution/bao-li-sou-suo-ji-yi-hua-sou-suo-dong-tai-gui-hua-/)

## 2. 按平方数来分割整数

279\. Perfect Squares(Medium)

[Leetcode](https://leetcode.com/problems/perfect-squares/description/) / [力扣](https://leetcode-cn.com/problems/perfect-squares/description/)

题目描述：For example, given n = 12, return 3 because 12 = 4 + 4 + 4; given n = 13, return 2 because 13 = 4 + 9.

### 方法一：递归+记忆化搜索

* 单独递归---超时
* 递归 + 记忆化搜索     （自顶向下）

```java
class Solution {
    int[] memory;
    public int numSquares(int n) {
        memory = new int[n + 1];
        for(int i=0;i<=n;i++){
            // 初始为最大值,这样后边才能比较最小值
            memory[i] = Integer.MAX_VALUE;
        }
        return count(n);
    }
    public int count(int n){
        // 说明上一个递归传下来的 n - i*i = 0 说明n就是一个完全平方数
        if(n == 0) return 0; 

        // 记忆化的核心
        if(memory[n] !=  Integer.MAX_VALUE) 
            return memory[n]; //memory[n]不是初始值,直接返回之前计算好的
        
		// 找memory[n]最小的情况：
        // 当前memory[n]就是最小，或者通过之前计算的memory[n-i*i] + 1(完全平方数i*i)得到
        for(int i=1;i*i <= n;i++){
            // 找到最小值,状态转移方程
            memory[n] = Math.min(memory[n], 1 + count(n-i*i));
        }
        return memory[n];
    }
}
```

### 方法二：动态规划

* 记忆化搜索（自顶向下）----> 动态规划（自底向上）
* 如果`n`为`0`，则结果为`0`
* 初始化长度为`n+1`的数组`dp`，每个位置都为`0`
* 对数组进行遍历，下标为`i`，**每次都将当前数字先更新为最大的结果**，即`dp[i]=i`，比如`i=4`，**最坏结果**为`4=1+1+1+1`即为`4`个数字
* `状态转移方程：dp[i] = Math.min(dp[i],1+dp[i- j*j])`  ,`i`表示当前数字，`j*j`表示一个平方数
* 时间复杂度：$ O(n\sqrt n)$
* 空间复杂度：O(n)

```java
class Solution {
    public int numSquares(int n) {
        // dp[i] 为正整数i的最小分割
        int[] dp = new int[n + 1];
        dp[0] = 0;
        for(int i=1;i<=n;i++){
            //dp[i] = Integer.MAX_VALUE; // 初始化为极大值
            dp[i] = i;
            for(int j=1;j*j <= i;j++){
                // 状态转移方程
               dp[i] = Math.min(dp[i],1+dp[i- j*j]);
            }
        }
        return dp[n];
    }
}
```

## 3. 分割整数构成字母字符串

91\. Decode Ways (Medium)

[Leetcode](https://leetcode.com/problems/decode-ways/description/) / [力扣](https://leetcode-cn.com/problems/decode-ways/description/)

题目描述：Given encoded message "12", it could be decoded as "AB" (1 2) or "L" (12).



### 方法一：动态规划

* 核心思想：当前序列 `s (0,i-1)` 在末尾加上一个字符 `s[i]`时，对于解码次数的影响

* `s[i] = '0'`

  * `s[i-1]`可以和 `s[i] = '0'`组合起来生成一个合法的数字 `[10,26]`,则：
    * `dp[i] = dp[i-2]`
  * `s[i-1]`不可以和 `s[i] = '0'`组合起来生成一个合法的数字 `[10,26]`,则：
    * **return 0 ,非法情况**

* `s[i] != '0'`

  * `s[i-1]`可以和 `s[i] != '0'`组合起来生成一个合法的数字 `[10,26]`,则：
    * `dp[i] = dp[i-2] + dp[i-1]`
  * `s[i-1]`不可以和 `s[i] != '0'`组合起来生成一个合法的数字 `[10,26]`,则：
    * `dp[i] = dp[i-1]` （`s[i] != '0'`自身可以转化，但是对于解码数无影响，例如：`133 + ‘2’`）

* 需要注意的地方(非法情况)：

  * | 非法情况                                                     |
    | ------------------------------------------------------------ |
    | **0**12   以0开头                                            |
    | 10**0**   当前字符为0，且当前字符和之前的字符组合“00”也不再(10,26)的范围 |
    | 33**0**   当前字符为0，且当前字符和之前的字符组合“30”也不再(10,26)的范围 |

```java
class Solution {
             public int numDecodings(String s) {
                 if(s == null || s.length() == 0) return 0;
                 if(s.charAt(0) == '0') return 0;    // 首字符为0无法转换,非法      
                 // dp[i] 到第i个字符时,所有的解码方式 
                 int[] dp = new int[s.length()+1];
                 dp[0]=1;dp[1]=1;
                 for(int i=2;i <= s.length();i++){
                    // i：第i个字符
                    int preNum = s.charAt(i-2) - '0'; // 前一个数字
                    int curNum = s.charAt(i-1) - '0'; // 当前数字
                    int num = preNum * 10 + curNum;   // 两个字符组合产生的数字
                    if(curNum == 0 && (preNum < 1 || preNum > 2)){
                        // 当前0无法转换，且和前一个字符组合-->'c0'也是非法
                        // 非法字符串，无法转换
                        return 0;
                    }
                    if(curNum == 0 && (preNum == 1 || preNum == 2)){
                        // 当前字符为0，无法转换
                        // 当前字符为0可以和之前的字符组合起来转换---dp[i-2]
                        // 所以当前的dp[i]的解码数就是dp[i-2],
                        // 第(i-1 和 i)个字符组合起来产生一个字符对于解码数无影响
                        dp[i] = dp[i-2];
                    }else if(10 <= num && num <= 26){
                        // (1)当前字符不为0,可以自己转换            dp[i-1]
                        // (2)当前字符可以和之前的字符组合起来转换    dp[i-2]
                        dp[i] = dp[i-1] + dp[i-2];
                    }else{
                        // 当前字符不为0,可以自己转换    dp[i-1]
                        // 不可以和之前的字符组合起来转换
                        dp[i] = dp[i-1]; 
                    }
                 }
                 return dp[s.length()];
             }
         }
}
```

### 方法二：动态规划 + 空间压缩

由于只用到了`dp[i-1] ，dp[i-2]`所以只用两个变量存起来，然后不断递推



# 最长递增子序列

已知一个序列 {S<sub>1</sub>, S<sub>2</sub>,...,S<sub>n</sub>}，取出若干数组成新的序列 {S<sub>i1</sub>, S<sub>i2</sub>,..., S<sub>im</sub>}，其中 i1、i2 ... im 保持递增，即新序列中各个数仍然保持原数列中的先后顺序，称新序列为原序列的一个  **子序列**  。

如果在子序列中，当下标 ix > iy 时，S<sub>ix</sub> > S<sub>iy</sub>，称子序列为原序列的一个  **递增子序列**  。

定义一个数组 dp 存储最长递增子序列的长度，dp[n] 表示以 S<sub>n</sub> 结尾的序列的最长递增子序列长度。对于一个递增子序列 {S<sub>i1</sub>, S<sub>i2</sub>,...,S<sub>im</sub>}，如果 im < n 并且 S<sub>im</sub> < S<sub>n</sub>，此时 {S<sub>i1</sub>, S<sub>i2</sub>,..., S<sub>im</sub>, S<sub>n</sub>} 为一个递增子序列，递增子序列的长度增加 1。满足上述条件的递增子序列中，长度最长的那个递增子序列就是要找的，在长度最长的递增子序列上加上 S<sub>n</sub> 就构成了以 S<sub>n</sub> 为结尾的最长递增子序列。因此 dp[n] = max{ dp[i]+1 | S<sub>i</sub> < S<sub>n</sub> && i < n} 。

因为在求 dp[n] 时可能无法找到一个满足条件的递增子序列，此时 {S<sub>n</sub>} 就构成了递增子序列，需要对前面的求解方程做修改，令 dp[n] 最小为 1，即：

<!--<div align="center"><img src="https://latex.codecogs.com/gif.latex?dp[n]=max\{1,dp[i]+1|S_i<S_n\&\&i<n\}" class="mathjax-pic"/></div> <br>-->

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/ee994da4-0fc7-443d-ac56-c08caf00a204.jpg" width="350px"> </div><br>
对于一个长度为 N 的序列，最长递增子序列并不一定会以 S<sub>N</sub> 为结尾，因此 dp[N] 不是序列的最长递增子序列的长度，需要遍历 dp 数组找出最大值才是所要的结果，max{ dp[i] | 1 <= i <= N} 即为所求。

## 1. 最长递增子序列

300\. Longest Increasing Subsequence (Medium)

[Leetcode](https://leetcode.com/problems/longest-increasing-subsequence/description/) / [力扣](https://leetcode-cn.com/problems/longest-increasing-subsequence/description/)

[详细题解](https://leetcode-cn.com/problems/longest-increasing-subsequence/solution/zui-chang-shang-sheng-zi-xu-lie-dong-tai-gui-hua-2/)

### 方法一：动态规划

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
         if(nums == null) return 0;
         // dp[i] 到第i个数时最长子序列的长度
         int[] dp = new int[nums.length];
         for(int i=0;i<nums.length;i++){
             dp[i] = 1;
         }
         for(int i=0;i<nums.length;i++){
             for(int j=0;j < i;j++){
                 if(nums[j] < nums[i]){
                     // 若dp[j]+1比较大，则表示在nums[j]之后接上 nums[i]会比之前的序列长
                     // 若dp[i]比较小，则表示nums[j]之后 不接上 nums[i](如果接上了反而会更小)
                     dp[i] = Math.max(dp[i],dp[j]+1);
                 }
             }
         }
         int max=0;
         for(int i=0;i<nums.length;i++){
             max = Math.max(max,dp[i]);
         }
         return max;                                                                                      
    }

```

### 方法二：动态规划 + 二分优化

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        // tails[i]：长度为i+1的上升子序列中的最小尾元素
        // 有可能有多个长度相同的子序列,保持尾部最小的情况下，更可能在接下来的查找中接上去
        // 利用了：数组内信息，数组坐标信息
        int[] tails = new int[nums.length];
        int maxlen = 0;
        for(int num : nums) {
            int i = 0, j = res;
            while(i < j) {
                // 这里的二分查找，配合下边的操作很精妙
                int m = i+(j-i)/2;
                if(tails[m] < num) 
                    i = m + 1;
                else 
                    j = m;
            }
            tails[i] = num; // 二分查找出来后,tails[i-1] < num < tails[i]
            if(maxlen == j) // j<=i
                maxlen++;
        }
        return maxlen;
    }
}
```



## 2. 一组整数对能够构成的最长链

646\. Maximum Length of Pair Chain (Medium)

[Leetcode](https://leetcode.com/problems/maximum-length-of-pair-chain/description/) / [力扣](https://leetcode-cn.com/problems/maximum-length-of-pair-chain/description/)

```html
Input: [[1,2], [2,3], [3,4]]
Output: 2
Explanation: The longest chain is [1,2] -> [3,4]
```

题目描述：对于 (a, b) 和 (c, d) ，如果 b < c，则它们可以构成一条链。

### 方法一：贪心算法

```java
class Solution {
    public int findLongestChain(int[][] pairs) {
        // 转换为求：最多的不重叠区间的个数
        // 核心思想：尽可能多的有不重叠的区间，所以取end小的区间,这样后边才有位置放其他的区间(其实也就是有更多的数对)
        // 1.定义排序：按照每个数对的end从小到大排序
        // 2.遍历：pre_end < cur_start 才可以进行数对连接
        Arrays.sort(pairs,(a,b)->a[1]-b[1]);
        int preEnd = Integer.MIN_VALUE;
        int count=0;
        for(int[] curPair : pairs){
            if(preEnd < curPair[0]){
                count++;
            	preEnd = curPair[1];
            }
        }
        return count;
    }
}
```

- 时间复杂度：O(NlogN)
- 空间复杂度：O(N)    排序空间复杂度，与语言有关

### 方法二：动态规划

* 动态规划

* **核心思想：**数对的start进行从小到大排序，在一个长度为 `k`，以 `pairs[j]` 结尾的数对链中，如果 `pairs[j][1] < pairs[i][0]`（数对 `pairs[j]`的 `end`比 数对  `pairs[i]`的`start`小），则以 `pairs[j]` 结尾的数对链之后接上一个`dp[i]`数对，使得数对链长度变为 `k+1`
  * 若 `k+1` > `dp[i]`,说明在`dp[j]`之后接上一个`dp[i]`数对能使得以`dp[i]`结尾的链更长
  * 若  `k+1` <= `dp[i]`,说明在`dp[j]`之后接上一个`dp[i]`数对能使得以`dp[i]`结尾的链更短(或者无变化)
* **状态转移方程：**`dp[i] = max(dp[i], dp[j] + 1)`
* **初始状态：**`dp数组全1`，表示每个数对单独作为链的结尾
* **状态更新：**根据数对的第一个数排序所有的数对，`dp[j]` 存储以 `pairs[j]` 结尾的最长链的长度。当 `j < i` 且 `pairs[j][1] < pairs[i][0]` 时，扩展数对链，更新 
* 时间复杂度：O(N^2)
* 空间复杂度：O(N)

```java
class Solution {
    public int findLongestChain(int[][] pairs) {
        // 1.定义排序：按照每个数对的start从小到大排序
        Arrays.sort(pairs,(a,b)->a[0]-b[0]);
        // dp[i],以第i个数对结尾的链的最大长度
        int[] dp = new int[pairs.length];
        // 2.初始状态
        Arrays.fill(dp,1); 
        for(int i=0;i<pairs.length;i++){
            for(int j=0;j<i;j++){
                if(pairs[j][1] <  pairs[i][0]){
                    // 3.转移方程
                    // 若dp[j]+1较大 ,则说明在dp[j]之后接上一个dp[i]数对能使得链更长
                    // 若dp[i]较大,这说明在dp[j]之后接上一个dp[i]数对反而使数对更短
                    dp[i] = Math.max(dp[i],dp[j]+1);
                }
            }
        }
        // 4.取dp数组中长度最大的
        int max = 0;
        for(int len : dp){
            max = Math.max(len,max);
        }
        return max;
    }
}
```





## 3. 最长摆动子序列

376\. Wiggle Subsequence (Medium)

[Leetcode](https://leetcode.com/problems/wiggle-subsequence/description/) / [力扣](https://leetcode-cn.com/problems/wiggle-subsequence/description/)

**[详细题解](https://leetcode-cn.com/problems/wiggle-subsequence/solution/bai-dong-xu-lie-by-leetcode/)**

```html
Input: [1,7,4,9,2,5]
Output: 6
The entire sequence is a wiggle sequence.

Input: [1,17,5,10,13,15,10,5,16,8]
Output: 7
There are several subsequences that achieve this length. One is [1,17,10,13,10,16,8].

Input: [1,2,3,4,5,6,7,8,9]
Output: 2
```

思路：

* **贪心思想**：寻找以当前`i`为起始点的升序 或 降序片段的末尾元素的下标 `[i,index]` (一个升序或者降序片段)

* **这样保证升序序列时找到最大元素，可以确保下一个元素是比其小的元素（这样就产生了摆动）**

* **这样保证降序序列时找到最小元素，可以确保下一个元素是比其大的元素（这样就产生了摆动）**

  * 若**前一个片段**是**升序片段**，则当前`nums[i]`必定是之前片段的**最大元素**，接下来的必有

    `nums[i] < nums[i+1]`，所以接下来找**降序片段**的末尾下标

  * 若**前一个片段**是**降序片段**，则当前`nums[i]`必定是之前片段的**最小元素**，接下来的必有

    `nums[i] < nums[i+1]`，所以接下来寻找升序片段

* 反正法：假设之前的片段为：升序片段，则`nums[i]`为之前升序片段的最后一个最大元素

  若 `nums[i] <= nums[i+1]`则，`nums[i+1]`才会为升序片段的最后一个最大元素，与假设矛盾，因此`nums[i]`必定是升序片段的最后一个最大元素，同理可证：降序片段也一样

* 时间复杂度：O（N）

* 空间复杂度：O（1）

```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        if(nums == null || nums.length == 0) return 0;
        int count = 1;
        int i = 0;
        while(i < nums.length - 1){
            if(nums[i] == nums[i+1]){
                // 1.开头元素相等情况不处理
                i++;
                continue;
            }
            // 2.贪心思想：寻找以当前i为起始点的升序 或 降序片段的末尾元素的下标

            // [i,index] (一个升序或者降序片段)
            int index = findAscOrDesSegmentLast(nums,i); 
            i = index;
            count++;
        }
        return count;
    }
    private int findAscOrDesSegmentLast(int[] nums,int start){
         // 传入下标为数组最后一个元素的下标时，直接返回
        if(start == nums.length-1)  return start; 

        int index=start;
        if(nums[start] <= nums[start+1]){
                // 当前元素 <= 后一个元素  所以是升序片段
                // 寻找升序序列的尾元素的index
                for(int i=start;i < nums.length-1;i++){
                    if(nums[i] <= nums[i+1]){
                        // 前一个元素 <= 后一个元素时
                        index = i+1;
                    }else{
                        break;
                    }
                }
        }else{
            // 当前元素 > 后一个元素  所以是降序片段
            // 寻找降序序列的尾元素的index
            for(int i=start;i < nums.length-1;i++){
                if(nums[i] >= nums[i+1]){
                    // 前一个元素 >= 后一个元素时
                    index = i+1;
                }else{
                    break;
                }
            }
        }
        return index;
    }
}
```

贪心代码简化：

```java
public class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums.length < 2)
            return nums.length;
        // prevDiff <= 0 代表前一个摆动是下降
        // prevDiff >= 0 代表前一个摆动是上升
        int prevDiff = nums[1] - nums[0];
        int count = 0;
        if(prevDiff != 0){
            count = 2;
        }else{
            count = 1;
        }
        for (int i = 2; i < nums.length; i++) {
            int diff = nums[i] - nums[i - 1];
            if ((diff > 0 && prevDiff <= 0) || (diff < 0 && prevDiff >= 0)) {
                //  前一个摆动是上升，当前摆动是下降
                // 或前一个摆动是下降，当前摆动是上升
                count++;
                prevDiff = diff;
            }
        }
        return count;
    }
}
```



# 最长公共子序列

对于两个子序列 S1 和 S2，找出它们最长的公共子序列。

定义一个二维数组 dp 用来存储最长公共子序列的长度，其中 `dp[i][j]` 表示 S1 的前 i 个字符与 S2 的前 j 个字符最长公共子序列的长度。考虑 S1<sub>i</sub> 与 S2<sub>j</sub> 值是否相等，分为两种情况：

- 当 S1<sub>i</sub>==S2<sub>j</sub> 时，那么就能在 S1 的前 i-1 个字符与 S2 的前 j-1 个字符最长公共子序列的基础上再加上 S1<sub>i</sub> 这个值，最长公共子序列长度加 1，即 `dp[i][j] = dp[i-1][j-1] + 1`。

- 当 S1<sub>i</sub> != S2<sub>j</sub> 时，此时最长公共子序列为 S1 的前 i-1 个字符和 S2 的前 j 个字符最长公共子序列，或者 S1 的前 i 个字符和 S2 的前 j-1 个字符最长公共子序列，取它们的最大者，**（关键：此处较难理解）**

  即 `dp[i][j] = max{ dp[i-1][j], dp[i][j-1] }。`  （此处类似矩阵最小路径和）

综上，最长公共子序列的状态转移方程为：

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/ecd89a22-c075-4716-8423-e0ba89230e9a.jpg" width="450px"> </div><br>

对于长度为 N 的序列 S<sub>1</sub> 和长度为 M 的序列 S<sub>2</sub>，`dp[N][M]` 就是序列 S<sub>1</sub> 和序列 S<sub>2</sub> 的最长公共子序列长度。

与最长递增子序列相比，最长公共子序列有以下不同点：

- 针对的是两个序列，求它们的最长公共子序列。
- 在最长递增子序列中，dp[i] 表示以 S<sub>i</sub> 为结尾的最长递增子序列长度，子序列必须包含 S<sub>i</sub> ；在最长公共子序列中，dp[i][j] 表示 S1 中前 i 个字符与 S2 中前 j 个字符的最长公共子序列长度，不一定包含 S1<sub>i</sub> 和 S2<sub>j</sub>。
- 在求最终解时，最长公共子序列中 `dp[N][M]` 就是最终解，而最长递增子序列中 dp[N] 不是最终解，因为以 S<sub>N</sub> 为结尾的最长递增子序列不一定是整个序列最长递增子序列，需要遍历一遍 dp 数组找到最大者。

## 1. 最长公共子序列

1143\. Longest Common Subsequence

[Leetcode](https://leetcode.com/problems/longest-common-subsequence/) / [力扣](https://leetcode-cn.com/problems/longest-common-subsequence/)

```java
    public int longestCommonSubsequence(String text1, String text2) {
        int n1 = text1.length(), n2 = text2.length();
        int[][] dp = new int[n1 + 1][n2 + 1];
        for (int i = 1; i <= n1; i++) {
            for (int j = 1; j <= n2; j++) {
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[n1][n2];
    }
```

# 0-1 背包

有一个容量为 N 的背包，要用这个背包装下物品的价值最大，这些物品有两个属性：体积 w 和价值 v。

定义一个二维数组 dp 存储最大价值，其中 `dp[i][j]` 表示前 i 件物品体积不超过 j 的情况下能达到的最大价值。设第 i 件物品体积为 w，价值为 v，根据第 i 件物品是否添加到背包中，可以分两种情况讨论：

- 第 i 件物品没添加到背包，总体积不超过 j 的前 i 件物品的最大价值就是总体积不超过 j 的前 i-1 件物品的最大价值，`dp[i][j] = dp[i-1][j]。`
- 第 i 件物品添加到背包中，`dp[i][j] = dp[i-1][j-w] + v。`

第 i 件物品可添加也可以不添加，取决于哪种情况下最大价值更大。因此，0-1 背包的状态转移方程为：

<!--<div align="center"><img src="https://latex.codecogs.com/gif.latex?dp[i][j]=max(dp[i-1][j],dp[i-1][j-w]+v)" class="mathjax-pic"/></div> <br>-->

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/8cb2be66-3d47-41ba-b55b-319fc68940d4.png" width="400px"> </div><br>
```java
// W 为背包总体积
// N 为物品数量
// weights 数组存储 N 个物品的重量
// values 数组存储 N 个物品的价值
public int knapsack(int W, int N, int[] weights, int[] values) {
    int[][] dp = new int[N + 1][W + 1];
    for (int i = 1; i <= N; i++) {
        int w = weights[i - 1], v = values[i - 1];
        for (int j = 1; j <= W; j++) {
            if (j >= w) {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - w] + v);
            } else {
                dp[i][j] = dp[i - 1][j];
            }
        }
    }
    return dp[N][W];
}
```

**空间优化**  

在程序实现时可以对 0-1 背包做优化。观察状态转移方程可以知道，前 i 件物品的状态仅与前 i-1 件物品的状态有关，因此可以将 dp 定义为一维数组，其中 dp[j] 既可以表示 dp[i-1][j] 也可以表示 dp[i][j]。此时，

<!--<div align="center"><img src="https://latex.codecogs.com/gif.latex?dp[j]=max(dp[j],dp[j-w]+v)" class="mathjax-pic"/></div> <br>-->

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/9ae89f16-7905-4a6f-88a2-874b4cac91f4.jpg" width="300px"> </div><br>
因为 dp[j-w] 表示 dp[i-1][j-w]，因此不能先求 dp[i][j-w]，防止将 dp[i-1][j-w] 覆盖。也就是说要先计算 dp[i][j] 再计算 dp[i][j-w]，在程序实现时需要按倒序来循环求解。

```java
public int knapsack(int W, int N, int[] weights, int[] values) {
    int[] dp = new int[W + 1];
    for (int i = 1; i <= N; i++) {
        int w = weights[i - 1], v = values[i - 1];
        for (int j = W; j >= 1; j--) {
            if (j >= w) {
                dp[j] = Math.max(dp[j], dp[j - w] + v);
            }
        }
    }
    return dp[W];
}
```

**无法使用贪心算法的解释**  

0-1 背包问题无法使用贪心算法来求解，也就是说不能按照先添加性价比最高的物品来达到最优，这是因为这种方式可能造成背包空间的浪费，从而无法达到最优。考虑下面的物品和一个容量为 5 的背包，如果先添加物品 0 再添加物品 1，那么只能存放的价值为 16，浪费了大小为 2 的空间。最优的方式是存放物品 1 和物品 2，价值为 22.

| id | w | v | v/w |
| --- | --- | --- | --- |
| 0 | 1 | 6 | 6 |
| 1 | 2 | 10 | 5 |
| 2 | 3 | 12 | 4 |

**变种**  

- 完全背包：物品数量为无限个

- 多重背包：物品数量有限制

- 多维费用背包：物品不仅有重量，还有体积，同时考虑这两种限制

- 其它：物品之间相互约束或者依赖

## 1. 划分数组为和相等的两部分

416\. Partition Equal Subset Sum (Medium)

[Leetcode](https://leetcode.com/problems/partition-equal-subset-sum/description/) / [力扣](https://leetcode-cn.com/problems/partition-equal-subset-sum/description/)

```html
Input: [1, 5, 11, 5]

Output: true

Explanation: The array can be partitioned as [1, 5, 5] and [11].
```



[详细题解](https://leetcode-cn.com/problems/partition-equal-subset-sum/solution/0-1-bei-bao-wen-ti-xiang-jie-zhen-dui-ben-ti-de-yo/)

### 方法一：递归（超时）

```java
class Solution {
    public boolean canPartition(int[] nums) {
        if(nums == null) return false;
        int sum = 0;
        for(int i=0;i<nums.length;i++){
            sum += nums[i];
        }
        if(sum % 2 == 1) return false;
        return judge(nums,sum/2,0);
    }
    private boolean judge(int[] nums,int sum,int index){
        if(index >= nums.length) return false;
        if(nums[index] > sum) return false;
        if(nums[index] == sum) return true;
        return judge(nums,sum - nums[index],index+1)  // 选取当前的元素
                || judge(nums,sum,index+1);           // 不选取当前的元素
    }
}
```

### 方法二：动态规划

可以看成一个背包大小为 sum/2 的 0-1 背包问题。

```java
class Solution {
    public boolean canPartition(int[] nums) {
        if(nums == null) return false;
        int len = nums.length;
        int sum = 0;
        for(int i=0;i<len;i++){
            sum += nums[i];
        }
        //全部数的和为奇数,肯定不能划分成两个和相等的子集
        // 若sum1 = sum2,则：sum = sum1 + sum2 = 2 * sum1 (所以sum为偶数)
        if((sum & 1) == 1) return false;
        // 转换成背包问题
        int target = sum/2;
        boolean[][] dp = new boolean[len][target + 1]; //初始化全false
        // i从1开始,表示第一个数绝对不选,那么剩下的数肯定还是可以判断相加起来是否==target
      	// 若这个数组划分可以得到target,那么不选择第i=0个数
        // 则表示划分到第i=0个数所在的集合肯定不能成功,而没有第i=0个数的另一个集合能成功
        // 而则另一个不包含第i=0个数的集合必定会成功,因为假设了划分可行
        // 如果相加得不到target,那么取不取第i=0个数也无所谓了
        for(int i=1;i < len;i++){
            for(int j=target;j >= 0;j--){
                // 直接从上一行先把结果抄下来，然后再修正
                dp[i][j] = dp[i - 1][j];
                if (nums[i] == j) {
                    dp[i][j] = true;
                    continue;
                }
                if (nums[i] < j) {
                    // 此处只用到了dp数组中的上一行的第j个 和 上一行dp数组中的第j - nums[i]个
                    dp[i][j] = dp[i - 1][j] || dp[i - 1][j - nums[i]个];
                }
            }
        }
        return dp[len-1][target];
    }
}
```

### 方法三：动态规划+状态压缩

```java
class Solution {
    public  boolean canPartition(int[] nums) {
        int len = nums.length;
        if (len == 0) return false;
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        // 特判：如果是奇数，就不符合要求
        if ((sum & 1) == 1) return false;
        int target = sum / 2;
        // 创建二维状态数组，行：物品索引，列：容量（包括 0）
        boolean[] dp = new boolean[target + 1];
        // 再填表格后面几行
        for (int i = 1; i < len; i++) {
            for (int j = target; j >= 0; j--) {
                if (nums[i] == j) {
                    dp[j] = true;
                    continue;
                }
                if (nums[i] < j) {
                    // 状态压缩 dp[i][j] = dp[i - 1][j] || dp[i - 1][j - nums[i]个];
                    // 上边只用到了dp数组中的上一行的第j个 和 上一行dp数组中的第j - nums[i]个
                    // 因此可以只使用一个一维dp数组,
                    // 为了保证“上一行dp数组中的第j - nums[i]个”的状态不被覆盖,可以从后向前遍历dp
                    dp[j] = dp[j] || dp[j - nums[i]];
                }
            }
        }
        return dp[target];
    }
}
```

### 方法四：BitSet方法

* 利用一个二进制位的位置表示数组中出现的数，
  * 例如：

    | 0      | 0     | 4     | 3     |
    | ------ | ----- | ----- | ----- |
    | 二进制 | 00001 | 10000 | 01000 |

初始化为bit = 00000000000  

表示数组[2, 4, 5, 9]

分别在bit得第[2, 4, 4, 9]置1

bit = 1000110101（当出现16 大于初始的bit位数时，将会自动扩充）

**比如计算[3,1,4]的得所有可能的和(数字的组合相加)结果**

**初始化bit0 = 0001**

1. **考虑3**

bit1 = bit0 | bit0 << 3

bit1 = 0001 | 1000   = 1001   

**bit0说明：不加3， bit0<<3 说明：0+3  两种可能，**

**所以两个可能的相加组合---不加3或者加3：两个可能结果的 按位或运算 则能得到表示两种相加结果的二进制**

2. **考虑1**

bit2 = bit1 | bit1 << 3

bit2 =  1001 | 1001000 = 1001001 

可以得到所有可能得结果为0 、1、3和 4（位置0 、1、3和 4上面为1）

3. **考虑4**

bit3 = bit2 | bit2 << 4

bit = 1001001 | 10010010000  =  1001001011001

```java
//问题： java的BitSet好像无法移位？？？？


class Solution {
    public  boolean canPartition(int[] nums) {
         // 每个数组中的元素不会超过 100  数组的大小不会超过 200  100*200 = 20000
        BitSet bits = new BitSet(20001);
        bits.set(0);
        int sum = 0;
        for(int num:nums){
            bits = bits | (bits << num);
            sum = sum + num;
        }
        int target = 1;
        target = target << (sum/2);
        if(target & bits != 0){
            return true;
        }
        return false;
    }
}
```



## 2. 改变一组数的正负号使得它们的和为一给定数

494\. Target Sum (Medium)

[Leetcode](https://leetcode.com/problems/target-sum/description/) / [力扣](https://leetcode-cn.com/problems/target-sum/description/)

```html
Input: nums is [1, 1, 1, 1, 1], S is 3.
Output: 5
Explanation:

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

There are 5 ways to assign symbols to make the sum of nums be target 3.
```

数组中的数都是非负数，`>=0`

该问题可以转换为 Subset Sum 问题，从而使用 0-1 背包的方法来求解。

可以将这组数看成两部分，P 和 N，其中 **P 使用正号**，**N 使用负号**，有以下推导：

```html
                  sum(P) - sum(N) = target
sum(P) + sum(N) + sum(P) - sum(N) = target + sum(P) + sum(N)
                       2 * sum(P) = target + sum(nums)
```

因此**只要找到一个子集**，**令它们都取正号**，并且**和等于 (target + sum(nums))/2**，就证明存在解。（转换成了上一题） （即：在数组中对于第 `i` 个数可以采取加或者不加）

```java
public int findTargetSumWays(int[] nums, int S) {
    int sum = computeArraySum(nums);
    if (sum < S || (sum + S) % 2 == 1) { 
        //sum < S说明数组全部数都取+加起来也比S小，肯定不可能
        //  (sum + S) % 2 == 1  由于： 2 * sum(P) = target + sum(nums)
        // 所以 target + sum(nums)肯定是一个偶数
        return 0;
    }
    int W = (sum + S) / 2;
    int[] dp = new int[W + 1];
    dp[0] = 1; //对于0,采取什么数都不加的操作
    for(int i=0;i<nums.length;i++){
        int num = nums[i];
        for(int j=W;j>=num;j--){
            // dp[j]:对于第i个数采取不加操作
            // dp[j-num]对于第i个数采取相加操作
            dp[j] = dp[j]: + dp[j-num];
        }
    }
    return dp[W];
}

private int computeArraySum(int[] nums) {
    int sum = 0;
    for (int num : nums) {
        sum += num;
    }
    return sum;
}
```

DFS 解法：

```java
public int findTargetSumWays(int[] nums, int S) {
    return findTargetSumWays(nums, 0, S);
}

private int findTargetSumWays(int[] nums, int start, int S) {
    if (start == nums.length) {
        return S == 0 ? 1 : 0;
    }
    return findTargetSumWays(nums, start + 1, S + nums[start])
            + findTargetSumWays(nums, start + 1, S - nums[start]);
}
```

## 3. 01 字符构成最多的字符串

474\. Ones and Zeroes (Medium)

[Leetcode](https://leetcode.com/problems/ones-and-zeroes/description/) / [力扣](https://leetcode-cn.com/problems/ones-and-zeroes/description/)

```html
Input: Array = {"10", "0001", "111001", "1", "0"}, m = 5, n = 3
Output: 4

Explanation: There are totally 4 strings can be formed by the using of 5 0s and 3 1s, which are "10","0001","1","0"
```

### 方法一：递归（超时）

```java
class Solution {
    int[][] count;

    public int findMaxForm(String[] strs, int m, int n) {
        count01(strs);
        return maxForm(strs,0,m,n);
    }
    private void count01(String[] strs){
        // 分别计算字符串数组中每个字符串中所含0,1的个数
        // 保存到二维数组中,arr[i][0] 保存第i个字符串中0的个数
        // 保存到二维数组中,arr[i][1] 保存第i个字符串中1的个数
        int[][] arr = new int[strs.length][2];
        for(int i=0; i < strs.length;i++){
            String s = strs[i];
            for(int j=0;j < s.length();j++){
                char c = s.charAt(j);
                if(c == '0'){
                    arr[i][0] = arr[i][0] + 1;
                }else{
                    arr[i][1] = arr[i][1] + 1;
                }
            }
        }
        this.count = arr;
    }
    private int maxForm(String[] strs,int index,int m,int n){
        // 递归,寻找最大的组合：对于第index个字符串，有两种操作：选择 或者 不选择
        if(index == strs.length) return 0;
        int noSelect=0,select=0;
        // 不选择第index个
        noSelect = maxForm(strs,index + 1,m, n);
        if(count[index][0] <= m && count[index][1] <= n){
            // 选择第index个
             select = 1 + maxForm(strs,index + 1,m-count[index][0], n - count[index][1]);
        }
        // 返回其中最大的
        return Math.max(select,noSelect);
    }
}
```

### 方法二：动态规划（三维数组）

```java
class Solution {

    public int findMaxForm(String[] strs, int m, int n) {
        if (strs == null || strs.length == 0) {
            return 0;
        }
        int[][][] dp = new int[strs.length + 1][m + 1][n + 1];
        for (int p = 0;p<strs.length;p++) {
            int zeros=0,ones=0;
            String str = strs[p];
            for(int k = 0;k<str.length();k++){
                if(str.charAt(k) == '0'){
                    zeros++;
                }else {
                    ones++;
                }
            }
            for(int i=0;i<= m;i++){
                for(int j=0;j<=n;j++){
                    // 先把上一行抄下来
                    dp[p+1][i][j] = dp[p][i][j];
                    if(i>= zeros && j>= ones){
                      dp[p+1][i][j] = Math.max(dp[p][i][j],dp[p][i - zeros][j-ones] + 1);
                    }
                }
            }
        }
        return dp[strs.length][m][n];
    }
}
```

### 方法三：动态规划（状态压缩成--二维数组）

**由于只用到了上一行，因此可以使用滚动数组，也可以从后向前赋值（一般状态压缩的考虑）。**

这是一个多维费用的 0-1 背包问题，有两个背包大小，0 的数量和 1 的数量。

```java
public int findMaxForm(String[] strs, int m, int n) {
    if (strs == null || strs.length == 0) {
        return 0;
    }
    int[][] dp = new int[m + 1][n + 1];
    for (String s : strs) {    // 每个字符串只能用一次
        int ones = 0, zeros = 0;
        for (char c : s.toCharArray()) {
            if (c == '0') {
                zeros++;
            } else {
                ones++;
            }
        }
        //实际上是三维dp--->优化成二维dp，所以需要从后往前,否则被覆盖
        for (int i = m; i >= zeros; i--) {
            for (int j = n; j >= ones; j--) {
                dp[i][j] = Math.max(dp[i][j], dp[i - zeros][j - ones] + 1);
            }
        }
    }
    return dp[m][n];
}
```

## 4. 找零钱的最少硬币数

322\. Coin Change (Medium)

[Leetcode](https://leetcode.com/problems/coin-change/description/) / [力扣](https://leetcode-cn.com/problems/coin-change/description/)

```html
Example 1:
coins = [1, 2, 5], amount = 11
return 3 (11 = 5 + 5 + 1)

Example 2:
coins = [2], amount = 3
return -1.
```

题目描述：给一些面额的硬币，要求用这些硬币来组成给定面额的钱数，并且使得硬币数量最少。硬币可以重复使用。

- 物品：硬币
- 物品大小：面额
- 物品价值：数量



### 方法一：递归+剪枝

```java
import java.util.Arrays;

class Solution {
    int min = Integer.MAX_VALUE;
    public int coinChange(int[] coins, int amount) {
        Arrays.sort(coins);
        countCoins(coins,amount,0,coins.length - 1);
        return min == Integer.MAX_VALUE?-1:min;
    }
    private void countCoins(int[] coins,int amount,int count,int index){
        if(amount == 0){
            if(count < min ){
                min = count;
            }
            return;
        }
        // 剪枝操作,尝试失败，不用进行后面的循环
        // 因为当前循环的i更小,使得下一个递归还剩余amount这样在下一层递归计算硬币反而会比min大
        if(index == -1|| amount/coins[index] + count >= min) return;
        // 循环选择硬币数
        for(int i = amount/(coins[index]);i >= 0;i--){
            countCoins(coins,amount - i * coins[index],count + i,index - 1);
        }
        return;
    }
```

### 方法二：完全背包

因为硬币可以重复使用，因此这是一个`完全背包问题`。完全背包只需要将 0-1 背包的逆序遍历 `dp` 数组改为正序遍历即可。

```java
public class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        for (int coin : coins) {
            for (int i = coin; i <= amount; i++) { //将逆序遍历改为正序遍历
                if (i == coin) {
                    dp[i] = 1;
                } else if (dp[i] == 0 && dp[i - coin] != 0) {
                    dp[i] = dp[i - coin] + 1;

                } else if (dp[i - coin] != 0) {
                    dp[i] = Math.min(dp[i], dp[i - coin] + 1);
                }
            }
        }
        return dp[amount] == 0 ? -1 : dp[amount];
    }
}
```

另一种写法

```java
public class Solution {
  public int coinChange(int[] coins, int amount) {
    int max = amount + 1;
    int[] dp = new int[amount + 1];
    // 填充最值保证下边的min过程的正确性,面值最小的硬币为1
    Arrays.fill(dp, max);
    dp[0] = 0;
    for (int i = 1; i <= amount; i++) { // 容量
      for (int j = 0; j < coins.length; j++) { // 硬币面值
        if (coins[j] <= i) {
            //dp[i - coins[j]] + 1:选取当前面值为coins[j]的一枚硬币
            //dp[i]： 不选择当前面值为coins[j]的一枚硬币
            dp[i] = Math.min(dp[i], dp[i - coins[j]] + 1);
        }
      }
    }
    // dp[amount] > amount 说明没有组合 返回-1 否则返回dp[amount]
    return dp[amount] > amount ? -1 : dp[amount];
  }
}
```

## 5. 找零钱的硬币数组合

518\. Coin Change 2 (Medium)

[Leetcode](https://leetcode.com/problems/coin-change-2/description/) / [力扣](https://leetcode-cn.com/problems/coin-change-2/description/)

```text-html-basic
Input: amount = 5, coins = [1, 2, 5]
Output: 4
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```

**完全背包问题**，使用 dp 记录可达成目标的组合数目。



```java
class Solution {
    public int change(int amount, int[] coins) {
        int[] dp = new int[amount + 1];
        dp[0] = 1;
        for (int coin : coins) {
            for(int i=coin;i<=amount;i++){
                dp[i] = dp[i] + dp[i-coin];
            }
        }
        return dp[amount];
    }
}
```

## 6. 字符串按单词列表分割

139\. Word Break (Medium)

[Leetcode](https://leetcode.com/problems/word-break/description/) / [力扣](https://leetcode-cn.com/problems/word-break/description/)

```html
s = "leetcode",
dict = ["leet", "code"].
Return true because "leetcode" can be segmented as "leet code".
```

dict 中的单词没有使用次数的限制，因此这是一个完全背包问题。

该问题涉及到字典中单词的使用顺序，也就是说物品必须按一定顺序放入背包中，例如下面的 dict 就不够组成字符串 "leetcode"：

```html
["lee", "tc", "cod"]
```

求解顺序的完全背包问题时，对物品的迭代应该放在最里层，对背包的迭代放在外层，只有这样才能让物品按一定顺序放入背包中。

```java
public class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        int n = s.length();
        boolean[] dp = new boolean[n + 1];
        dp[0] = true;
        for (int i = 1; i <= n; i++) { // 背包容量
            for (String word : wordDict) {
                int len = word.length();
                //s.substring(i - len, i) 后缀(0,i)的后缀字符串
                // 尝试将物品放入
                if (len <= i && word.equals(s.substring(i - len, i))) {
                    // dp[i - len]表示可以将当前物品放入，然后查看dp[i - len]状态看看该子问题是否满足
                    // dp[i]查看是否已经存在解
                    dp[i] = dp[i] || dp[i - len];
                }
            }
        }
        return dp[n];
    }
}
```

## 7. 组合总和

377\. Combination Sum IV (Medium)

[Leetcode](https://leetcode.com/problems/combination-sum-iv/description/) / [力扣](https://leetcode-cn.com/problems/combination-sum-iv/description/)

```html
nums = [1, 2, 3]
target = 4

The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)

Note that different sequences are counted as different combinations.

Therefore the output is 7.
```

![](https://pic.leetcode-cn.com/fa278029267fedeb06686b784bd322f16b2abf6b61987dc3b5257630570cd38f-377-1.png)

可以发现有很多的 **重叠子问题**，因此可以使用 **记忆化搜索 或者 动态规划**

### 方法一：递归+记忆化搜索（超时）

```java
class Solution {
    int[] memo;
    public int combinationSum4(int[] nums, int target) {
        if(target <= 0) return 0;
        memo = new int[target + 1];
        memo[0] = 1;
        return memoSearch(nums,target);
    }
    private int memoSearch(int[] nums,int target){
        if(target == 0) return 1;
        if(target < 0) return 0;
        if(memo[target] != 0) return memo[target];
        for(int i = 0;i < nums.length;i++){
            memo[target] += memoSearch(nums,target - nums[i]);
        }
        return memo[target];
    }
}
```

### 方法二：动态规划

递归+记忆化搜索（超时） （自顶向下）----> 自底向上的 **动态规划**

**重叠子问题的优化----动态规划**

```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        int[] dp = new int[target + 1];
        dp[0] = 1; // dp[0]=1：组成0，一个数都不选 所以dp[0]=1
        for(int i = 1;i <= target;i++){ // 容量
            for(int j = 0;j < nums.length;j++){
                // 枚举nums[j]
                if(i >= nums[j]){
                    // dp[i] + 子问题dp[i - nums[j]]的解
                    dp[i] = dp[i] + dp[i - nums[j]];
                }
            }
        }
        return dp[target];
    }
}

```

也可以**理解为完全背包问题**？？？：

- 原本的完全背包问题： [1,2,1] 和 [1,1,2]算是**1种**
- 当前问题有点**特殊**： [1,2,1] 和 [1,1,2]算是**2种**
- 其实这个题有点特殊，顺序不同的组合也要算一组，所以**枚举的顺序必须是**:  （？？？为什么）

```
for(int j=1;j<=target;j++)
	for(int i=0;i<n;i++)
```



# 股票交易

## 1. 需要冷却期的股票交易

309\. Best Time to Buy and Sell Stock with Cooldown(Medium)

[Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/) / [力扣](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)

题目描述：交易之后需要有一天的冷却时间。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/ffd96b99-8009-487c-8e98-11c9d44ef14f.png" width="300px"> </div><br>
```java
public int maxProfit(int[] prices) {
    if (prices == null || prices.length == 0) {
        return 0;
    }
    int N = prices.length;
    int[] buy = new int[N];
    int[] s1 = new int[N];
    int[] sell = new int[N];
    int[] s2 = new int[N];
    s1[0] = buy[0] = -prices[0];
    sell[0] = s2[0] = 0;
    for (int i = 1; i < N; i++) {
        buy[i] = s2[i - 1] - prices[i];
        s1[i] = Math.max(buy[i - 1], s1[i - 1]);
        sell[i] = Math.max(buy[i - 1], s1[i - 1]) + prices[i];
        s2[i] = Math.max(s2[i - 1], sell[i - 1]);
    }
    return Math.max(sell[N - 1], s2[N - 1]);
}
```

## 2. 需要交易费用的股票交易

714\. Best Time to Buy and Sell Stock with Transaction Fee (Medium)

[Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/description/) / [力扣](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/description/)

```html
Input: prices = [1, 3, 2, 8, 4, 9], fee = 2
Output: 8
Explanation: The maximum profit can be achieved by:
Buying at prices[0] = 1
Selling at prices[3] = 8
Buying at prices[4] = 4
Selling at prices[5] = 9
The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
```

题目描述：每交易一次，都要支付一定的费用。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/1e2c588c-72b7-445e-aacb-d55dc8a88c29.png" width="300px"> </div><br>
```java
public int maxProfit(int[] prices, int fee) {
    int N = prices.length;
    int[] buy = new int[N];
    int[] s1 = new int[N];
    int[] sell = new int[N];
    int[] s2 = new int[N];
    s1[0] = buy[0] = -prices[0];
    sell[0] = s2[0] = 0;
    for (int i = 1; i < N; i++) {
        buy[i] = Math.max(sell[i - 1], s2[i - 1]) - prices[i];
        s1[i] = Math.max(buy[i - 1], s1[i - 1]);
        sell[i] = Math.max(buy[i - 1], s1[i - 1]) - fee + prices[i];
        s2[i] = Math.max(s2[i - 1], sell[i - 1]);
    }
    return Math.max(sell[N - 1], s2[N - 1]);
}
```


## 3. 只能进行两次的股票交易

123\. Best Time to Buy and Sell Stock III (Hard)

[Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/description/) / [力扣](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/description/)

```java
public int maxProfit(int[] prices) {
    int firstBuy = Integer.MIN_VALUE, firstSell = 0;
    int secondBuy = Integer.MIN_VALUE, secondSell = 0;
    for (int curPrice : prices) {
        if (firstBuy < -curPrice) {
            firstBuy = -curPrice;
        }
        if (firstSell < firstBuy + curPrice) {
            firstSell = firstBuy + curPrice;
        }
        if (secondBuy < firstSell - curPrice) {
            secondBuy = firstSell - curPrice;
        }
        if (secondSell < secondBuy + curPrice) {
            secondSell = secondBuy + curPrice;
        }
    }
    return secondSell;
}
```

## 4. 只能进行 k 次的股票交易

188\. Best Time to Buy and Sell Stock IV (Hard)

[Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/description/) / [力扣](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/description/)

```java
public int maxProfit(int k, int[] prices) {
    int n = prices.length;
    if (k >= n / 2) {   // 这种情况下该问题退化为普通的股票交易问题
        int maxProfit = 0;
        for (int i = 1; i < n; i++) {
            if (prices[i] > prices[i - 1]) {
                maxProfit += prices[i] - prices[i - 1];
            }
        }
        return maxProfit;
    }
    int[][] maxProfit = new int[k + 1][n];
    for (int i = 1; i <= k; i++) {
        int localMax = maxProfit[i - 1][0] - prices[0];
        for (int j = 1; j < n; j++) {
            maxProfit[i][j] = Math.max(maxProfit[i][j - 1], prices[j] + localMax);
            localMax = Math.max(localMax, maxProfit[i - 1][j] - prices[j]);
        }
    }
    return maxProfit[k][n - 1];
}
```

# 字符串编辑

## 1. 删除两个字符串的字符使它们相等

583\. Delete Operation for Two Strings (Medium)

[Leetcode](https://leetcode.com/problems/delete-operation-for-two-strings/description/) / [力扣](https://leetcode-cn.com/problems/delete-operation-for-two-strings/description/)

```html
Input: "sea", "eat"
Output: 2
Explanation: You need one step to make "sea" to "ea" and another step to make "eat" to "ea".
```

可以转换为求两个字符串的最长公共子序列问题。

```java
public int minDistance(String word1, String word2) {
    int m = word1.length(), n = word2.length();
    int[][] dp = new int[m + 1][n + 1];
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            } else {
                dp[i][j] = Math.max(dp[i][j - 1], dp[i - 1][j]);
            }
        }
    }
    return m + n - 2 * dp[m][n];
}
```

## 2. 编辑距离

72\. Edit Distance (Hard)

[Leetcode](https://leetcode.com/problems/edit-distance/description/) / [力扣](https://leetcode-cn.com/problems/edit-distance/description/)

```html
Example 1:

Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation:
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')
Example 2:

Input: word1 = "intention", word2 = "execution"
Output: 5
Explanation:
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')
```

题目描述：修改一个字符串成为另一个字符串，使得修改次数最少。一次修改操作包括：插入一个字符、删除一个字符、替换一个字符。

```java
public int minDistance(String word1, String word2) {
    if (word1 == null || word2 == null) {
        return 0;
    }
    int m = word1.length(), n = word2.length();
    int[][] dp = new int[m + 1][n + 1];
    for (int i = 1; i <= m; i++) {
        dp[i][0] = i;
    }
    for (int i = 1; i <= n; i++) {
        dp[0][i] = i;
    }
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1];
            } else {
                dp[i][j] = Math.min(dp[i - 1][j - 1], Math.min(dp[i][j - 1], dp[i - 1][j])) + 1;
            }
        }
    }
    return dp[m][n];
}
```

## 3. 复制粘贴字符

650\. 2 Keys Keyboard (Medium)

[Leetcode](https://leetcode.com/problems/2-keys-keyboard/description/) / [力扣](https://leetcode-cn.com/problems/2-keys-keyboard/description/)

题目描述：最开始只有一个字符 A，问需要多少次操作能够得到 n 个字符 A，每次操作可以复制当前所有的字符，或者粘贴。

```
Input: 3
Output: 3
Explanation:
Intitally, we have one character 'A'.
In step 1, we use Copy All operation.
In step 2, we use Paste operation to get 'AA'.
In step 3, we use Paste operation to get 'AAA'.
```

```java
public int minSteps(int n) {
    if (n == 1) return 0;
    for (int i = 2; i <= Math.sqrt(n); i++) {
        if (n % i == 0) return i + minSteps(n / i);
    }
    return n;
}
```

```java
public int minSteps(int n) {
    int[] dp = new int[n + 1];
    int h = (int) Math.sqrt(n);
    for (int i = 2; i <= n; i++) {
        dp[i] = i;
        for (int j = 2; j <= h; j++) {
            if (i % j == 0) {
                dp[i] = dp[j] + dp[i / j];
                break;
            }
        }
    }
    return dp[n];
}
```






<div align="center"><img width="320px" src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/githubio/公众号二维码-2.png"></img></div>
