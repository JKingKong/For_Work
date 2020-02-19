7<!-- GFM-TOC -->
* [1. 分配饼干](#1-分配饼干)
* [2. 不重叠的区间个数](#2-不重叠的区间个数)
* [3. 投飞镖刺破气球](#3-投飞镖刺破气球)
* [4. 根据身高和序号重组队列](#4-根据身高和序号重组队列)
* [5. 买卖股票最大的收益](#5-买卖股票最大的收益)
* [6. 买卖股票的最大收益 II](#6-买卖股票的最大收益-ii)
* [7. 种植花朵](#7-种植花朵)
* [8. 判断是否为子序列](#8-判断是否为子序列)
* [9. 修改一个数成为非递减数组](#9-修改一个数成为非递减数组)
* [10. 子数组最大的和](#10-子数组最大的和)
* [11. 分隔字符串使同种字符出现在一起](#11-分隔字符串使同种字符出现在一起)
<!-- GFM-TOC -->


保证每次操作都是局部最优的，并且最后得到的结果是全局最优的。

# 1. 分配饼干

455\. Assign Cookies (Easy)

[Leetcode](https://leetcode.com/problems/assign-cookies/description/) / [力扣](https://leetcode-cn.com/problems/assign-cookies/description/)

```html
Input: grid[1,3], size[1,2,4]
Output: 2
```

题目描述：每个孩子都有一个满足度 grid，每个饼干都有一个大小 size，只有饼干的大小大于等于一个孩子的满足度，该孩子才会获得满足。求解最多可以获得满足的孩子数量。

1. 给一个孩子的饼干应当尽量小并且又能满足该孩子，这样大饼干才能拿来给满足度比较大的孩子。
2. 因为满足度最小的孩子最容易得到满足，所以先满足满足度最小的孩子。

在以上的解法中，我们只在每次分配时饼干时选择一种看起来是当前最优的分配方法，但无法保证这种局部最优的分配方法最后能得到全局最优解。我们假设能得到全局最优解，并使用反证法进行证明，即假设存在一种比我们使用的贪心策略更优的最优策略。如果不存在这种最优策略，表示贪心策略就是最优策略，得到的解也就是全局最优解。

证明：假设在某次选择中，贪心策略选择给当前满足度最小的孩子分配第 m 个饼干，第 m 个饼干为可以满足该孩子的最小饼干。假设存在一种最优策略，可以给该孩子分配第 n 个饼干，并且 m < n。我们可以发现，经过这一轮分配，贪心策略分配后剩下的饼干一定有一个比最优策略来得大。因此在后续的分配中，贪心策略一定能满足更多的孩子。也就是说不存在比贪心策略更优的策略，即贪心策略就是最优策略。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/e69537d2-a016-4676-b169-9ea17eeb9037.gif" width="430px"> </div><br>
```java
public int findContentChildren(int[] grid, int[] size) {
    if (grid == null || size == null) return 0; //排除null情况
    // 从小到大排列
    Arrays.sort(grid);
    Arrays.sort(size);
    int gi = 0, si = 0;
    while (gi < grid.length && si < size.length) {
        if (grid[gi] <= size[si]) {
            // 孩子吃最合适的饼干
            gi++;
        }
        si++;
    }
    return gi;
}
```

不太的方法O(N^2):

```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        if (grid == null || size == null) return 0;
        int count = 0; //已经吃的数量 和 s开始的下标,[0,count)都是被吃掉的饼干
        for(int i=0;i<g.length;i++){
            //第i个孩子
            int properIndex = -1;
            int min = Integer.MAX_VALUE;
            // 寻找最合适的饼干
            for(int j=count;j < s.length;j++){
                if(g[i] <= s[j] && s[j] < min){
                    // 寻找最合适的饼干
                    min= s[j];
                    properIndex = j;
                }
            }
            if(properIndex != -1){
                int tmp = s[count];
                s[count] = s[properIndex];
                s[properIndex] = tmp;
                count++;
            }
        }
        return count;
    }
}
```

# 2. 不重叠的区间个数 *

435\. Non-overlapping Intervals (Medium)

[Leetcode](https://leetcode.com/problems/non-overlapping-intervals/description/) / [力扣](https://leetcode-cn.com/problems/non-overlapping-intervals/description/)

```html
Input: [ [1,2], [1,2], [1,2] ]

Output: 2

Explanation: You need to remove two [1,2] to make the rest of intervals non-overlapping.
```

```html
Input: [ [1,2], [2,3] ]

Output: 0

Explanation: You don't need to remove any of the intervals since they're already non-overlapping.
```

题目描述：计算让一组区间不重叠所需要移除的区间个数。

**关键**：

* **先计算最多能组成的不重叠区间个数，然后用区间总个数减去不重叠区间的个数**。
* 

### 方法一：

**先计算最多能组成的不重叠区间个数，然后用区间总个数减去不重叠区间的个数**。

在每次选择中，区间的**结尾最为重要**，选择的区间结尾越小，留给后面的区间的空间越大，那么后面能够选择的区间个数也就越大。

**按区间的结尾进行排序，每次选择结尾最小，并且和前一个区间不重叠的区间**。

```java
public int eraseOverlapIntervals(int[][] intervals) {
    if (intervals.length == 0) {
        return 0;
    }
    // 按照每个区间的end排序，由小到大
    Arrays.sort(intervals,(o1,o2) ->{
        if(o1[1] < o2[1]){
            return -1;
        }else if(o1[1] > o2[1]){
            return 1;
        }else{
            return 0;
        }
    });
    int cnt = 1;
    int end = intervals[0][1];
    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i][0] < end) {
            //发生重叠，当前所选区间的start < 目标区间的end            
            continue;
        }
        end = intervals[i][1];
        cnt++;
    }
    return intervals.length - cnt;
}
```

使用 lambda 表示式创建 Comparator 会导致算法运行时间过长，如果注重运行时间，可以修改为普通创建 Comparator 语句：

```java
//最简形式：
Arrays.sort(intervals, new Comparator<int[]>() {
    @Override
    public int compare(int[] o1, int[] o2) {
        return o1[1] - o2[1];
    }
});
```

### 方法二(类似模拟)

![](https://pic.leetcode-cn.com/311ab170f5b301b3a97ebb5be89317e5c9ca47be5117b5bfbf3083ceec7346b4-image.png)

**按照区间开头进行排序，从小到大**

情况一**

当前考虑的两个区间不重叠：

在这种情况下，不移除任何区间，将 prevprev 赋值为后面的区间，移除区间数量不变。

**情况二**

两个区间重叠，后一个区间的终点在前一个区间的终点之前。

这种情况下，我们可以简单地只用后一个区间。这是显然的，因为后一个区间的长度更小，可以留下更多的空间（AA 和 BB），容纳更多的区间。因此， prevprev 更新为当前区间，移除区间的数量 + 1。

**情况三**

两个区间重叠，后一个区间的终点在前一个区间的终点之后。

这种情况下，我们用贪心策略处理问题，直接移除后一个区间。为了理解这种做法的正确性，请看下图，该图包含了所有可能的情况。从图可以清楚地看出，选择前移区间总会得到更好的结果。因此，prevprev 不变，移除区间的数量 + 1

```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        if(intervals == null || intervals.length <= 1) return 0;
        int prev=0;
        int count = 0;
        //按照每个区间的 start 排序，由小到大
        Arrays.sort(intervals,(o1,o2) ->{
           return o1[0] - o2[0];
        });
        // 模拟当前区间i与之前区间prev的相遇情况---三种
        for(int i = 1; i<intervals.length;i++){
            if(intervals[prev][1] >= intervals[i][1]){
                //区间重叠，情况1   prev: [1,4]  i:[2,3] 或者[2,4]这种情况
                prev = i;
                count++; // 移除prev指向的区间
            }else if(intervals[prev][1] > intervals[i][0] && intervals[prev][1] < intervals[i][1]){
                //区间重叠，情况2 prev: [1,4]  i:[2,6]这种情况
                count++;//移除i指向的区间
            }else if(intervals[prev][1] <= intervals[i][0]){
                // 区间未重叠,不需要移除  prev: [1,4]  i:[4,6]或者[5,7]这种情况
                prev=i;
            }
        }
        return count;
    }
}

```

# 3. 投飞镖刺破气球

452\. Minimum Number of Arrows to Burst Balloons (Medium)

[Leetcode](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/description/) / [力扣](https://leetcode-cn.com/problems/minimum-number-of-arrows-to-burst-balloons/description/)

```
Input:
[[10,16], [2,8], [1,6], [7,12]]

Output:
2
```

题目描述：气球在一个水平数轴上摆放，可以重叠，飞镖垂直投向坐标轴，使得路径上的气球都被刺破。求解最小的投飞镖次数使所有气球都被刺破。

### 方法一：

也是**计算不重叠的区间个数**，不过和 Non-overlapping Intervals 的**区别**在于，[1, 2] 和 [2, 3] 在本题中算是重叠区间。

```java
public int findMinArrowShots(int[][] points) {
    if (points.length == 0) {
        return 0;
    }
    Arrays.sort(points, Comparator.comparingInt(o -> o[1]));
    int cnt = 1, end = points[0][1];
    for (int i = 1; i < points.length; i++) {
        if (points[i][0] <= end) {
            continue;
        }
        cnt++;
        end = points[i][1];
    }
    return cnt;
}
```

### 方法二：(类似模拟)

```java
class Solution {
    public int findMinArrowShots(int[][] intervals) {
        if(intervals == null) return 0;
        if(intervals.length <= 1) return intervals.length;
        //按照每个区间的 start 排序，由小到大
        Arrays.sort(intervals,(o1,o2) ->{
           return o1[0] - o2[0];
        });
        int count = 0; //飞镖数
        int end = intervals[0][1]; // 重叠气球的最小end
        // 注意边界重叠 [1,2] [2,3]  在2处也算重叠
        //由于排过序，下一个气球i的起始点肯定>=上一个气球的起始点
        for(int i = 1; i<intervals.length;i++){
            if(end >= intervals[i][1]){
                //区间重叠，情况1---覆盖,
                end = intervals[i][1];
            }else if(end >= intervals[i][0] && end <= intervals[i][1]){
                //区间重叠，情况2--交叠
                end = end;
            }else if(end < intervals[i][0]){
                // 区间未与之前的气球重叠 投飞镖射爆气球
                end = intervals[i][1];
                count++;
            }
        }
        count++; // 处理最后的气球，有可能是重叠的没戳爆 或者 最后一个单独的气球
        return count;
    }
}
```







# 4. 根据身高和序号重组队列 * 

406\. Queue Reconstruction by Height(Medium)

[Leetcode](https://leetcode.com/problems/queue-reconstruction-by-height/description/) / [力扣](https://leetcode-cn.com/problems/queue-reconstruction-by-height/description/)

```html
Input:
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

Output:
[[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]
```

题目描述：一个学生用两个分量 (h, k) 描述，h 表示身高，k 表示排在前面的有 k 个学生的身高比他高或者和他一样高。

```java
class Solution {
      /**
     * 解题思路：先排序再插入
     * 1.排序规则：按照先H高度降序，如果高度相同,则后按K个数升序排序
     * 2.遍历排序后的数组，根据K插入到K的位置上
     *
     * 核心思想：高个子先站好位，矮个子插入到K位置上，前面肯定有K个高个子，矮个子再插到前面也满足K的要求
     *
     * @param people
     * @return
     */
    public int[][] reconstructQueue(int[][] people) {
        Arrays.sort(people,(o1,o2)->{
            if(o1[0] == o2[0]){ 
                return o1[1] - o2[1];
            }else{
                return o2[0] - o1[0];
            }
        });
        LinkedList<int[]> list = new LinkedList<>();
        for (int[] i : people) {
            //表示第i个高个 插到第k=i[1]位置上
            list.add(i[1], i);
        }
        return list.toArray(new int[list.size()][2]);
    }
}
```

# 5. 买卖股票最大的收益

121\. Best Time to Buy and Sell Stock (Easy)

[Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/) / [力扣](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/description/)

题目描述：一次股票交易包含买入和卖出，只进行一次交易，求最大收益。

只要记录前面的最小价格，将这个最小价格作为买入价格，然后将当前的价格作为售出价格，查看当前收益是不是最大收益。

```java
public int maxProfit(int[] prices) {
    int n = prices.length;
    if (n == 0) return 0;
    int preMin = prices[0];
    int max = 0;
    for (int i = 1; i < n; i++) {
        if (preMin > prices[i]) preMin = prices[i];
        else max = Math.max(max, prices[i] - preMin);
    }
    return max;
}
```


# 6. 买卖股票的最大收益 II

122\. Best Time to Buy and Sell Stock II (Easy)

[Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/) / [力扣](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/description/)

题目描述：可以进行多次交易，多次交易之间不能交叉进行，可以进行多次交易。

对于 [a, b, c, d]，如果有 a <= b <= c <= d ，那么最大收益为 d - a。而 d - a = (d - c) + (c - b) + (b - a) ，因此当访问到一个 prices[i] 且 prices[i] - prices[i-1] > 0，那么就把 prices[i] - prices[i-1] 添加到收益中。

```java
public int maxProfit(int[] prices) {
    int profit = 0;
    for (int i = 1; i < prices.length; i++) {
        if (prices[i] > prices[i - 1]) {
            profit += (prices[i] - prices[i - 1]);
        }
    }
    return profit;
}
```


# 7. 种植花朵 * 

605\. Can Place Flowers (Easy)

[Leetcode](https://leetcode.com/problems/can-place-flowers/description/) / [力扣](https://leetcode-cn.com/problems/can-place-flowers/description/)

```html
Input: flowerbed = [1,0,0,0,1], n = 1
Output: True
```

题目描述：flowerbed 数组中 1 表示已经种下了花朵。花朵之间至少需要一个单位的间隔，求解是否能种下 n 朵花。

```java
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {
        if(n <= 0) return true;
        if(flowerbed == null) return false;
        int pre = 0,next = 0;
        for(int i=0;i < flowerbed.length;i++){
            if(flowerbed[i] == 1){
                // 当前位置已经有花则不可以在此地种，直接进入下一次循环
                continue;
            }
            if(i == 0){
                // 处理边界情况i==0时
                // i == 0的情况,0之前肯定是不影响是否能种，所以pre=0表示i-1位置没花
               pre = 0;
            }else{
                // i!=0时
                pre = flowerbed[i-1]; 
            }
            if(i == flowerbed.length - 1){
                // 处理边界情况i==len - 1时
                // i == len - 1的情况,len - 1之后肯定是不影响是否能种，所以next=0表示len位置没花
                next = 0;
            }else{
                //i!=len-1时
                next = flowerbed[i+1];
            }
            if(pre == 0 && next == 0){
                //当前位置i种了一朵花
                n--;
                // 
                i++;//下一个i+1的位置肯定不能种花  <----原本flowerbed[i] = 1;
            }
        }
        return n <= 0?true:false;
    }
}
```

* **注意考虑边界**

# 8. 判断是否为子序列

392\. Is Subsequence (Medium)

[Leetcode](https://leetcode.com/problems/is-subsequence/description/) / [力扣](https://leetcode-cn.com/problems/is-subsequence/description/)

```html
s = "abc", t = "ahbgdc"
Return true.
```

```java
public boolean isSubsequence(String s, String t) {
    int index = -1;
    for (char c : s.toCharArray()) {
        index = t.indexOf(c, index + 1);//在t的[index+1,len)上寻找，出现字符c的第一个下标
        if (index == -1) {
            //t上不存在字符c则：不为子序列
            return false;
        }
    }
    return true;
}
```

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        if(s == null || t == null) return false;
        int j = 0;
        for(int i=0;i < t.length();i++){
            if(j < s.length() && s.charAt(j) == t.charAt(i)){
                //匹配一个,j++去匹配下一个
                j++;
            }
        }
        return s.length() == j;
    }
}
```

# 9. 修改一个数成为非递减数组 *

665\. Non-decreasing Array (Easy)

[Leetcode](https://leetcode.com/problems/non-decreasing-array/description/) / [力扣](https://leetcode-cn.com/problems/non-decreasing-array/description/)

```html
Input: [4,2,3]
Output: True
Explanation: You could modify the first 4 to 1 to get a non-decreasing array.
```

题目描述：判断一个数组是否能只修改一个数就成为非递减数组。

在出现nums[i - 1] > nums[i] 时，需要考虑的是应该修改数组的哪个数，使得本次修改能使 i 之前的数组成为非递减数组，并且   **不影响后续的操作**  。

**优先考虑缩小** nums[i - 1] = nums[i]，因为**如果修改 nums[i] = nums[i - 1] 的话**，那么 nums[i] 这个数会变大，就有可能比 nums[i + 1] 大，从而影响了后续操作。

**特殊的情况处理**就是 nums[i - 2] > nums[i]，修改 nums[i - 1] = nums[i] 不能使数组成为非递减数组，只能修改 **nums[i] = nums[i - 1]**。

| 4,2,3   | "1",2,3   | TRUE |
| ------- | --------- | ---- |
| 5,6,2,7 | 5,6,"6",7 | true |
| 1,6,2,7 | 1,"2",2,7 | true |
|         |           |      |



```java
class Solution {
    public boolean checkPossibility(int[] nums) {
        if(nums == null) return false;
        int change = 0;
        for(int i=1;i<nums.length && change < 2;i++){
            if(nums[i-1] <= nums[i]){
                //满足非递减情况
                continue;
            }
            change++;
            //下边的 nums[i-1] > nums[i]
            // i-2比i大，改nums[i] = nums[i-1]
            if(i-2 >= 0 && nums[i-2] > nums[i]){
                //放大 nums[i] 特殊的情况处理nums[i - 2] > nums[i]
                nums[i] = nums[i-1];
            }else{
                // 缩小 nums[i-1]  ----- 优先考虑
                nums[i-1] = nums[i];
            }
        }
        return change <= 1;
    }
}
```



# 10. 子数组最大的和**

53\. Maximum Subarray (Easy)

[Leetcode](https://leetcode.com/problems/maximum-subarray/description/) / [力扣](https://leetcode-cn.com/problems/maximum-subarray/description/)

```html
For example, given the array [-2,1,-3,4,-1,2,1,-5,4],
the contiguous subarray [4,-1,2,1] has the largest sum = 6.
```

### 方法一：动态规划

**思路**

* 动态规划的是首先对数组进行遍历，**当前最大连续子序列和为 sum**，结果为 maxSubSum
* 如果 **sum > 0**，则说明 sum 对结果有增益效果，则 **sum 保留**并加上当前遍历数字
* 如果 **sum <= 0**，则说明 sum 对结果无增益效果，**sum舍弃**，则 sum 直接更新为当前遍历数字
* 每次比较 **sum** 和 **maxSubSum**的大小，将最大值置为**maxSubSum**，遍历结束返回结果
* 时间复杂度：O(n)

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int maxSubSum = nums[0];
        int sum = 0;
        for(int num: nums) {
            if(sum > 0) {
                sum += num;
            } else {
                sum = num;
            }
            maxSubSum = Math.max(maxSubSum, sum);
        }
        return maxSubSum;
    }
}
```

### 方法二贪心

该算法通用且简单：遍历数组并在每个步骤中更新：

- 当前元素
- 当前元素位置的最大和
- 迄今为止的最大和

```java
class Solution {
    public int maxSubArray(int[] nums) {
        if(nums == null || nums.length < 1) return 0;
        int maxSum = nums[0];
        int curSum = nums[0];
        for(int i=1;i<nums.length;i++){
            curSum = Math.max(nums[i],curSum + nums[i]);
            maxSum = Math.max(maxSum,curSum);
        }
        return maxSum;
    }   
}
```



```java
class Solution {
    public int maxSubArray(int[] nums) {
        if(nums == null || nums.length < 1) return 0;
        int maxSum = nums[0];
        int tmpSum = 0;
        for(int i=0;i<nums.length;i++){
            // 全负数--？取最小负数，全正数，有正数负数混合
            // tmpSum加当前
            tmpSum += nums[i];
            maxSum = Math.max(maxSum,tmpSum);
            if(tmpSum <= 0){
                //因为[0,i]的最大子序列和已经计算出为maxSum,
                //当前tmpSum = f(0,i-1) + nums[i] <= 0,说明当前 f(0,i+1) = tmpSum + nums[i+1]  <= nums[i+1]
                //因此重新开始计算tmpSum,
                tmpSum = 0;
            }
        }
        return maxSum;
    }   
}
```

### 方法三分治********

```java
class Solution {
    public int maxSubArray(int[] nums) {
        if(nums == null || nums.length < 1) return 0;       
        return maxSubArrayPart(nums, 0, nums.length-1);
    }
    
    private int maxSubArrayPart(int[] nums,int left,int right){
        if(left == right){
            // 返回当前
            return nums[left];
        }
        int mid = left + (right - left) / 2;
        /*-------------此处的分治思想最为关键，不要去深究细节，整体考虑--------------*/
        // 分治，求左半部分最大子序和
        int leftSum = maxSubArrayPart(nums, left, mid);
        // 分治，求右半部分最大子序和
        int rightSum = maxSubArrayPart(nums, mid + 1, right);
        // 分治，求穿过中间时的最大子序和(  向左<--mid +  mid+1-->向右 )----此处的考虑很精妙
        int crossSum = crossSumFun(nums,left, mid ,right);
        return Math.max(leftSum,Math.max(rightSum,crossSum)); //取最大： leftSum ,crossSum, rightSum
    }
    private int crossSumFun(int[] nums,int left,int mid,int right){
        // 左右两边合起来求解，从中间mid开始    向左<---mid   mid+1-->向右
        int leftSum=Integer.MIN_VALUE;
        int rightSum=Integer.MIN_VALUE;
        int tmpSum = 0;
        for(int i=mid;i>=left;i--){ // 向左<---mid
            tmpSum += nums[i];
            if(tmpSum > leftSum){
                leftSum = tmpSum;
            }
        }
        tmpSum = 0;
        for(int i=mid+1;i<=right;i++){ //mid+1-->向右
            tmpSum += nums[i];
            if(tmpSum > rightSum){
                rightSum = tmpSum;
            }
        }
        return leftSum + rightSum;  // 求和：crossSum
    }

}
```



# 11. 分隔字符串使同种字符出现在一起***

763\. Partition Labels (Medium)  //第一个字符 S[0],查找[1,len)上是否存在字符s[0],存在则说明

[Leetcode](https://leetcode.com/problems/partition-labels/description/) / [力扣](https://leetcode-cn.com/problems/partition-labels/description/)

解题思路：

1. 某个字符c，在S中出现的最后一个下标，存入charLastIndex数组
2. 

```java
class Solution {
    public List<Integer> partitionLabels(String S) {
        List<Integer> list = new ArrayList<Integer>();
        if(S == null || S.length() < 1) return list;
        int len = S.length();
        // 某个字符c，在S中出现的最后一个下标
        int[] charLastIndex = new int[26]; 
        for(int i=0;i < len;i++){
            char curChar = S.charAt(i);
            charLastIndex[charIndex(curChar)] = i;
        }

        // 当前片段的第一个字符的下标
        int curSegmentFirstIndex = 0; 
        // 当前片段的最后一个字符的下标(会动态改变)
        int curSegmentLastIndex = 0;  
        while(curSegmentFirstIndex < len){
            curSegmentLastIndex = curSegmentFirstIndex;
            // 处理每个段
            // 使得当前段的所有字符都在一起,即：段"ababcbaca"中的所有字符都不会在其它段出现
            // 核心思想：贪心，将S中在当前段出现的所有字符都纳入当前段，"axbxxxaxxxxb"
            for(int i=curSegmentFirstIndex; i < len && i <= curSegmentLastIndex; i++){
                //当前字符
                char curChar = S.charAt(i);
                //当前字符的最后一个下标
                int curCharLastIndex = charLastIndex[charIndex(curChar)];
                //当前字符的最后一个下标 比 当前段的最后一个下标大，则更新
                if(curCharLastIndex > curSegmentLastIndex){
                    curSegmentLastIndex = curCharLastIndex;
                }
            }
            //当前片段的长度加入到list中
            list.add(curSegmentLastIndex - curSegmentFirstIndex + 1); 
            // curSegmentFirstIndex下一个段的首个字符的下标
            curSegmentFirstIndex = curSegmentLastIndex + 1;
        }
        return list;
    }
    private int charIndex(char c){
        //计算某个字符的index  0--25
        return c - 'a';
    }
}
```






<div align="center"><img width="320px" src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/githubio/公众号二维码-2.png"></img></div>
