class Solution {
private:
    vector<int> orig;
    vector<int> newNums;
public:
    Solution(vector<int>& nums) {
        orig = nums;
        newNums = nums;
        srand(time(NULL));
    }
    
    /** Resets the array to its original configuration and return it. */
    vector<int> reset() {
        return orig;
    }
    
    /** Returns a random shuffling of the array. */
    vector<int> shuffle() {
        int tmp;
        for(int i=0;i<){
            int random_index = rand() % newNums.size();
            tmp = newNums[i];
            newNums[i] = newNums[random_index];
            newNums[random_index] = tmp;
        }
        return newNums;
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(nums);
 * vector<int> param_1 = obj->reset();
 * vector<int> param_2 = obj->shuffle();
    */

#  LeetCode刷题

#### 题目

#### 题目分析：

1. 

##### 解法一：

**解题思路：**

1. 

```C++

```

##### 解法二：

**解题思路：**

1. 

```C++

```

##### 收获：

1. 





## include<ctype.h>

* **int isalnum(char c)**  
  * 判断一个字符**是否为数字或者字母**，也就是说判断一个字符是否属于**a~z||A~Z||0~9**
* **int isalpha(char c)**
  * 判断一个字符**是否为字母**，如果是字符则返回非零，否则返回零。
* int isdigit(int c)
  * 判断一个字符是否为数字
* **int islower(char c)**
* **int isupper(char c)**
* int tolower(int c)
* int toupper(int c)

## 技巧：

### 数组初始化

```
//未赋值
int a[1000] = {0};里边全都是0
int a[1000];   里边全都是0
int a[1000] = {100};  里边依旧全都是0

```



### KMP

核心：

1. 

```C++
class Solution {
public:
    int strStr(string haystack, string needle) {
        if(needle == "") return 0;
        int hlen = haystack.size();
        int nlen = needle.size();
        int next[nlen+1] = {0};
        getNext(needle,next);
        int index = search(haystack,needle,next);
        return index;
    }
    int * getNext(string b,int* next)
    {
        int j=0;
        next[0]=next[1]=0;
        for(int i=1;i<len;i++)//i表示字符串的下标，从1 开始
        {//j在每次循环开始都表示next[i]的值，同时也表示需要比较的下一个位置
            while(j>0&&b[i]!=b[j])j=next[j];
            if(b[i]==b[j])
                j++;
            next[i+1]=j;
        }
        return next;
    }
    int search(string original, string find, int* next) {
        int j = 0;
        for (int i = 0; i < original.size(); i++) {
            while (j > 0 && original[i]!= find[j])
                j = next[j];
            if (original[i] == find[j])
                j++;
            if (j == find.size()) {
                //第一个匹配成功就返回
                return i - j + 1;
                // 注释掉return 
                // System.out.println("成功匹配的子串的起始下标 " + (i - j + 1));
                // j = 0;   // 重置j = 0;
            }
        }
        return -1;
    }
};
```

### # 链表

* 双指针，前一个curNode先走N步，后一个preNode才开始走
* 快慢指针，slow指针一次走一步，fast 指针一次走两步 (解决环链表问题)
* 头插法----链表翻转
* 尾插法

### 树

* 层次遍历
* 先序遍历
* 中序遍历
* 后序遍历（左 右 根  有些特殊的题需要右 左 根---左 右 根）
* 双指针 （左子树指针，右指数指针）
* 与层数有关可以递归向下传递当前层数或者下一层数
* 二分法（使用数组构造二叉搜索树 等）
* 二叉搜索树的节点值区间特性
* **递归解**  与  **迭代解**(需要栈或者队列)

### 数组

* 从后往前、从前往后
* 有序数组的二分法（二分查找）
* 数组/字符串合并，**从后往前 **遍历、合并

### 二分法

* ​          中点写法，不会溢出  mid = i + (j - i)/2;   (int i, j, mid;)
  * 会溢出的写法(int i, j, mid;)          mid = (i+j)/2   两个int相加可能会溢出

### 位运算

* 获取二进制数的每一位

  ```C++
  //例如32位 int
  int n = 4;  //
  for(int i=0;i<32;i++){
      //res保存当前位, (n>>i) n的二进制右移i位，而后与1进行与运算即可得到第i位二进制数是什么
      int res = ((n>>i) & 1); 
  }
  ```

  

### hash_set



### hash_map



### Set(红黑树)

* Set去重

```
Set<int> s;
s.insert(value);
```

### map(红黑树)

在C++中 map的底层实现是红黑树O(lgN)，如果时间复杂度要求严格或者想要更快的速度就使用#include<hash_map> - O(1)

* 判断key是否存在

```C++
map<int,int> m;
map<int,int>::iterator iter;
iter.find(key);
if(iter == m.end()){
    cout << "key不存在" << endl;
}else if(iter != m.end()){
    cout << "key:" << iter->first << endll;
    cout << "value:" << iter->second << endll;
}
```

* 遍历

```
iter = m.begin();
iter++;
iter != end();
```

### stringstream

* 字符-->数字

  ```C++
  #include<string>
  #include<sstream>
  
  int num;
  stringstream ss;
  string s="123";
  ss << s;
  ss >> num;
  //重用stringstream对象需要调用clear()方法，否则转换无效
  ss.clear();
  ```

* 数字-->字符

  ```c++
  #include<string>
  #include<sstream>
  
  int num = 123;
  stringstream ss;
  string s;
  ss << num;
  ss >> s;
  //重用stringstream对象需要调用clear()方法，否则转换无效
  ss.clear();
  ```

  

### 数组

####  旋转数组 （Rotate Array）

##### 题目

给定一个数组，将数组中的元素向右移动 *k* 个位置，其中 *k* 是非负数。

**示例 1:**

```
输入: [1,2,3,4,5,6,7] 和 k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右旋转 1 步: [7,1,2,3,4,5,6]
向右旋转 2 步: [6,7,1,2,3,4,5]
向右旋转 3 步: [5,6,7,1,2,3,4]
```

**示例 2:**

```
输入: [-1,-100,3,99] 和 k = 2
输出: [3,99,-1,-100]
解释: 
向右旋转 1 步: [99,-1,-100,3]
向右旋转 2 步: [3,99,-1,-100]
```

**说明:**

- 尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。
- 要求使用空间复杂度为 O(1) 的原地算法。

##### 解法一：

解题思路：新增一个数组arr，源数组的旋转后得到一个index，index%n 即为原数组旋转过后在新数组的下标

```C++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        long long len = nums.size();
        vector<int> arr(len);
        for(int i = 0;i < len;i++){
            arr[(i+k)%len] = nums[i];
        }
        for(int i = 0;i < len;i++){
            nums[i] = arr[i];
        }
    }
};
```

##### 解法二：

解题思路：使用一个变量tmp来代替arr的功能

```c++
条件：
k=2
1
1 2 3 4 5 6 7
第一次：
    3
1 2 1 4 5 6 7
第二次：
        5
1 2 1 4 3 6 7
第三次：
			7
1 2 1 4 3 6 5
第四次：
  2			
1 7 1 4 3 6 5
第五次：
	  4
1 7 1 2 3 6 5
第六次：
		  6
1 7 1 2 3 4 5
第七次(最终)：
1
6 7 1 2 3 4 5
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        long long len = nums.size();
        if(k % len == 0 || len == 0){
            //说明经过旋转之后元素还是在原位，所以等价于不操作
            //或者
            //数组长度为0，不需要操作
            return;
        }
        int a = nums[0];
        int index = 0;
        int start = 0;
        int tmp;    // 作为中间变量，用以交换 a与nums[index]
        for(int i=0;i<len;i++){//循环表示一共移动7个数字
            index = (index + k) % len;
            tmp = nums[index];
            nums[index] = a;
            a = tmp;
            if(index == start){
                // 说明回到了数组的初始位置，如果不加以处理，
                //就会这样会导致一直在0 0+2 2+2 4+2 ... 0重复移动,其他元素都没有移动到
                // 例如： [1 2 3 4 5 6] k=2
                start++;    //+1 构建一个新的初始位置  
                index++;
                a = nums[index]; // a等于新的起点的，元素值
            }
        }
    }
};

```





#### 两个数组交集II

##### 题目

给定两个数组，编写一个函数来计算它们的交集。

**示例 1:**

```
输入: nums1 = [1,2,2,1], nums2 = [2,2]
输出: [2,2]
```

**示例 2:**

```
输入: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出: [4,9]
```

**说明：**

- 输出结果中每个元素出现的次数，应与元素在两个数组中出现的次数一致。
- 我们可以不考虑输出结果的顺序。

##### 解法1：

```C++
解题思路：通过map数据结构解决问题
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        vector<int> result;
        map<int,int> m;
        long long len1 = nums1.size(),len2=nums2.size();
        map<int,int>::iterator iter;
        for(int i=0;i<len1;i++){
            iter = m.find(nums1[i]);
            if(iter != m.end()){
                m[nums1[i]] += 1;
            }else{
                m[nums1[i]] = 1;
            }
        }
        
        for(int j=0;j<len2;j++){
            iter = m.find(nums2[j]);
            if(iter != m.end()){
                if(iter->second > 0){
                     result.push_back(nums2[j]);
                     m[nums2[j]] -= 1;
                }
            }
        }
        return result;
    }
};
```

**进阶：**

1. 如果给定的数组已经排好序呢？你将如何优化你的算法？
2. 如果 *nums1* 的大小比 *nums2* 小很多，哪种方法更优？
3. 如果 *nums2* 的元素存储在磁盘上，磁盘内存是有限的，并且你不能一次加载所有的元素到内存中，你该怎么办？

(3题条件不互相关联，各自独立)



#### 买卖股票的最佳时机 II

##### 题目

给定一个数组，它的第 *i* 个元素是一支给定股票第 *i* 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

**注意：**你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

**示例 1:**

```
输入: [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。
```

**示例 2:**

```
输入: [1,2,3,4,5]
输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
```

**示例 3:**

```
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```

##### 解法一：

```C++
解题思路：贪心算法，总是做出在当前看来是最好的选择，不从整体最优上加以考虑，也就是说，只关心当前最优解
我们要算的是利润，要有利润，自然要有一次交易。
所以我们就说说prices[1]，即是第一天股票价格。按照贪心策略，不关心以后，我们只关心当前利益。第0天买入，花费prices[0]，第一天卖出，得到prices[1]，那么我们的收获就是profit = prices[1] - prices[0],那么有两种情况

（1）当profit > 0 时，赶紧买入卖出，能赚一笔是一笔，苍蝇再小也是肉嘛 

（2）当profit <= 0 时，再买入卖出的话，那就是傻了，白费力气不说，还亏钱。

class Solution {
public:
    int maxProfit(vector<int>& prices) {
        long long sum = 0;
        long long len = prices.size();
        for(int i=1;i<len;i++){
            int tmp = prices[i] - prices[i-1];
            if(tmp > 0){
                sum += tmp;
            }
        }
        return sum;
    }
};
```

##### 收获

* 贪心算法

#### 有效的数独

##### 题目

判断一个 9x9 的数独是否有效。只需要**根据以下规则**，验证已经填入的数字是否有效即可。

1. 数字 `1-9` 在每一行只能出现一次。
2. 数字 `1-9` 在每一列只能出现一次。
3. 数字 `1-9` 在每一个以粗实线分隔的 `3x3` 宫内只能出现一次。

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

上图是一个部分填充的有效的数独。

数独部分空格内已填入了数字，空白格用 `'.'` 表示。

**示例 1:**

```
输入:
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
输出: true
```

**示例 2:**

```
输入:
[
  ["8","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
输出: false
解释: 除了第一行的第一个数字从 5 改为 8 以外，空格内其他数字均与 示例1 相同。
     但由于位于左上角的 3x3 宫内有两个 8 存在, 因此这个数独是无效的。
```

**说明:**

- 一个有效的数独（部分已被填充）不一定是可解的。
- 只需要根据以上规则，验证已经填入的数字是否有效即可。
- 给定数独序列只包含数字 `1-9` 和字符 `'.'` 。
- 给定数独永远是 `9x9` 形式的。

##### 解法一：

```C++
解题思路： set<char> 与 模拟法
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        set<char> s;
        int len;
        // 判断每行符不符合规则(即：0-9是否重复)
        for(int i=0;i<9;i++){
            for(int j=0;j<9;j++){
                if(board[i][j] !='.'){
                    s.insert(board[i][j]);
                    len++;
                }
            }
            if(s.size() != len){
                return false;
            }else{
                s.clear();
                len = 0;
            }
        }
        // 判断每列符不符合规则(即：0-9是否重复)
        for(int j=0;j<9;j++){
            for(int i=0;i<9;i++){
                if(board[i][j] !='.'){
                    s.insert(board[i][j]);
                    len++;
                }
            }
            if(s.size() != len){
                return false;
            }else{
                s.clear();
                len = 0;
            }
        }
        // 判断九个3x3小正方形符不符合规则(即：0-9是否重复)
        for(int i=0;i<9;i=i+3){
            for(int j=0;j<9;j=j+3){
                for(int ii=0;ii<3;ii++){
                    for(int jj=0;jj<3;jj++){
                        if(board[i+ii][j+jj] !='.'){
                            s.insert(board[i+ii][j+jj]);
                            len++;
                        }
                    }
               }
               if(s.size() != len){
                 return false;
               }else{
                 s.clear();
                 len = 0;
               }
            }
        }
        return true;
    }
};
```

##### 解法二：

**上边代码的简化**

```C++
//上边代码的简化
// index = i/3 * 3 + j/3   每个3x3小正方形(共9个) 映射到一个标号上(0--8)
 public boolean isValidSudoku(char[][] board) {
        boolean[][] markRow = new boolean[9][10];
        boolean[][] markCol = new boolean[9][10];
        boolean[][] markBlock = new boolean[9][10];
        // 多定义两个数组可以省去初始化操作
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                int index = i/3 * 3 + j/3; //关键点 小正方形映射到index标号
                if (board[j][i] != '.') {
                    int temp = board[j][i] - '0';
                    if (markRow[j][temp]) return false;
                    else markRow[j][temp] = true;
                    if (markCol[i][temp]) return false;
                    else markCol[i][temp] = true;
                    if (markBlock[index][temp]) return false;
                    else markBlock[index][temp] = true;
                }
            }
        }
        return true;
 }
```

##### 解法三：

**位运算优化上边解法二的代码**(好像用到了状态压缩)

<https://blog.csdn.net/qq_36326947/article/details/80266062>

```C++
 public boolean isValidSudoku(char[][] board) {
        int[][] mark = new int[3][9];
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                int index = i/3 * 3 + j/3;
                if (board[j][i] != '.') {
                    // 1左移？位，数独中最大数为9，2^8大大小于int的max
                    int temp = 1 << (board[j][i] - '1');
                    // 判断每列是否出现重复用temp，否则|上temp
                    if ((mark[0][j] & temp) > 0) return false;
                    else mark[0][j] |= temp;
                    // 每行
                    if ((mark[1][i] & temp) > 0) return false;
                    else mark[1][i] |= temp;
                    //每块
                    if ((mark[2][index] & temp) > 0) return false;
                    else mark[2][index] |= temp;
                }
            }
        }
        return true;
    }




//改进上边的代码 if那里

    public boolean isValidSudoku(char[][] board) {
        int[][] mark = new int[3][9];
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                int index = i/3 * 3 + j/3;
                if (board[j][i] != '.') {
                    int temp = 1 << (board[j][i] - '1');
                    if ((mark[0][j] & temp) > 0 || (mark[1][i] & temp) > 0 || (mark[2][index] & temp) > 0) return false;
                    mark[0][j] |= temp;
                    mark[1][i] |= temp;
                    mark[2][index] |= temp;
                }
            }
        }
        return true;


```



##### 收获

* **模拟法**
* 不断优化代码，是否能将冗余代码简化，空间换时间或者加空间简化冗余代码
* **映射法**，9个3x3的小矩形，每个都可以**映射**成为一个编号index=i/3 * 3 + j/3（0-8）







####   旋转图像

##### 题目

给定一个 *n* × *n* 的二维矩阵表示一个图像。

将图像顺时针旋转 90 度。

**说明：**

你必须在**原地**旋转图像，这意味着你需要直接修改输入的二维矩阵。**请不要**使用另一个矩阵来旋转图像。

**示例 1:**

```
给定 matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

原地旋转输入矩阵，使其变为:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

**示例 2:**

```
给定 matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

原地旋转输入矩阵，使其变为:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]
```

##### 解法一：

**解题思路**：

* 1、从外到内，一个大的矩形，可以看作多个矩形(从小到大)不断嵌套而成
  1为外的大矩形，2位内的小矩形 

  ```
  6  1  1  7 
  1  2  2  1   
  1  2  2  1
  9  1  1  8
  ```

  

* 2、smallMatrixRotate（）从每个小矩形的左上角开始，每轮次有四个元素成功交换，然后小矩形的左上角col + 1,继续轮换四个元素

  ```
  6  1  1  7 
  1  2  2  1   
  1  2  2  1
  9  1  1  8
  第一轮次的转换：
  9  1  1  6 
  1  2  2  1   
  1  2  2  1
  8  1  1  7
  ```

  

```C++

class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int len = matrix.size();
        // 有多少个小矩形
        int small_matrix_count = len/2;
        int start_row=0,start_col=0;
        int small_matrix_len;
        for(int k=1;k<=small_matrix_count;k++){
        	// 小矩形长
            small_matrix_len = len - (k - 1)*2;
            // 转换一个小矩形
            smallMatrixRotate(matrix,start_row,start_col,small_matrix_len);
           	// 进入下一个小矩形
            start_row += 1;
            start_col += 1;
        }
    }
    void smallMatrixRotate(vector<vector<int>>& matrix,int start_row,int start_col,int small_matrix_len){
        int old_i,old_j;
        int new_i,new_j;
        int a;
        int tmp;
        for(int i=start_col; i < start_col + small_matrix_len - 1; i++){
            // 第一层循环表示遍历小矩形的顶列
            old_i = start_row;
            old_j = i;
            a = matrix[old_i][old_j];
            for(int times=0;times < 4;times++){
                // 第二层循环表示开始4个元素的轮换
                //new_index
                new_i = old_j;
                new_j = matrix.size() - 1 - old_i;
                // swap
                tmp = matrix[new_i][new_j];
                matrix[new_i][new_j] = a;
                a = tmp;
                //new_index ---> old_index
                old_i = new_i;
                old_j = new_j;
            }
        }
    }
};
```

##### 解法二：

```C++
public void test() {
     int[][] test1 = new int[][]{
             {1, 2, 3},
             {4, 5, 6},
             {7, 8, 9}
     };

     rotate(test1);
     Assert.assertArrayEquals(new int[][]{
             {7, 4, 1},
             {8, 5, 2},
             {9, 6, 3}
     }, test1);
 }


 public void rotate(int[][] matrix) {

     int length = matrix.length;

     // 调换对角元素
     for (int i = 0; i < length; i++) {
         for (int j = 0; j < length - i; j++) {
             int tmp = matrix[i][j];
             matrix[i][j] = matrix[length - j - 1][length - i - 1];
             matrix[length - j - 1][length - i - 1] = tmp;
         }
     }

     // 调换列元素
     for (int i = 0; i < length; i++) {
         for (int j = 0; j < length / 2; j++) {
             int tmp = matrix[j][i];
             matrix[j][i] = matrix[length - j - 1][i];
             matrix[length - j - 1][i] = tmp;
         }
     }
 }
}
```

#### 字符串中的第一个唯一字符

##### 题目

给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。

**案例:**

```C++
s = "leetcode"
返回 0.

s = "loveleetcode",
返回 2.
```

**注意事项：**您可以假定该字符串只包含小写字母。

##### 解法一：

**解题思路：**

* 遍历字符串序列，用map保存：key-value(字符-index)，若同一个字符重复重复出现，则：置index = -1
* 遍历map，找出index最小的一个，如果index全部都是 - 1则返回-1

```C++
//想更快可以使用hash_map，但leetcode对于hash_map支持很差
class Solution {
public:
    int firstUniqChar(string s) {
        // c++ 中 map是红黑树
        map<char,int> m;
        map<char,int>::iterator iter;
        int len  = s.size();
        for(int i=0;i<len;i++){
            iter = m.find(s[i]);
            if(iter == m.end()){
                //字母第一次出现  key-value(字符-index)
                m[s[i]] = i;
            }else if(iter != m.end()){
                //字母出现重复
                 m[s[i]] = -1;
            }
        }
        //初始时min_index为字符串的长度,比最大的index大 1
        int min_index = len;
        iter = m.begin();
        while(iter != m.end()){
            if(iter->second != -1 && iter->second < min_index)
                min_index = iter->second;
            iter++;
        }
        return min_index == len?-1:min_index;
    }
};
```

##### 方法二：

```c++
class Solution {
public:
    int firstUniqChar(string s) {
        int len = s.size();
        //times[0]==0:代表字母“a”没有出现0次，times[0]==1:代表字母“a”出现了重复1次
        //times[2]==0:代表字母“c”没有出现0次，times[2]==3:代表字母“c”出现了重复3次
        int times[26] = {0};   
        //first_index[0] = xxx：代表字母“a”第一次出现的下标为：xxx
        //first_index[2] = xxx：代表字母“c”第一次出现的下标为：xxx
        int first_index[26] = {0};
        for(int i=0; i<s.size();i++){ 
            if(first_index[s[i] - 'a'] == 0){
                //==26说明还未有此字母
                first_index[s[i] - 'a'] = i;
            }
            times[s[i] - 'a'] += 1; 
        }
        int min_index = len;
        for(int i=0;i<26;i++){
            if(times[i] == 1){
                // 只查看出现过一次的字母所对应的下标
                if(first_index[i] < min_index){
                    min_index = first_index[i];
                }
            }                
        }
        return min_index == len?-1:min_index;
    }
};
```

##### 收获

* 字母与数组下标的映射
* 迭代器遍历
* map的使用(底层是红黑树)（可以采用hash_map加速）
* 如果存在一些映射关系：比如字母与下标的映射，则可以使用数组代替map

#### 验证回文字符串

##### 题目

给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

**说明：**本题中，我们将空字符串定义为有效的回文串。

**示例 1: **       （看例子**和一般的求回文串不一样**）

```
输入: "A man, a plan, a canal: Panama"			
输出: true
```

**示例 2:**

```
输入: "race a car"
输出: false
```

##### 解法一：

**解题思路：**

* （1）i：从前向后扫，j：从后向前扫，
  * 如果：i 所对应的字符既不是字母也不是数字，则：i++; continue;
  * 如果：j 所对应的字符既不是字母也不是数字，则：j--; continue;
* （2），条件1满足，则在比较i 与 j对应的字符前，要将大写字母转换成小写字母再比较

```C++
class Solution {
public:
    bool isPalindrome(string s) {
        long long len = s.size();
        if(len == 0)
            return true;
        int j = len - 1;
        int i = 0;
        char a,b;
        while(i < j){
            if(!isDigit(s[i]) && !isLetter(s[i])){
                i++;
                continue;
            }
            if(!isDigit(s[j]) && !isLetter(s[j])){
                j--;
                continue;
            }
            a = s[i];
            b = s[j];
            if(isLetter(a))
                a = lowerLetter(a);
            if(isLetter(b))
                b = lowerLetter(b);
            if(a != b)
                return false;
            i++;
            j--;
        }
        return true;
    }
    bool isDigit(char c){
        if((c - '0'>=0) && (c - '0') <= 9){
            return true;
        }else 
            return false;
    }
    bool isLetter(char c){
        if(((c - 'a'>=0) && (c - 'a') <= 25)
          ||((c - 'A'>=0) && (c - 'A') <= 25)
          )
        {
            return true;
        }
        else return false;
    }
    char lowerLetter(char c){
        if((c - 'a'>=0) && (c - 'a') <= 25)
            return c;
        else if((c - 'A'>=0) && (c - 'A') <= 25)
            c = 'a' + (c - 'A');
        return c;
    
    }
};
```



##### 解法二：

**解题思路：**

- （1）先处理字符数组，字母与数字放在字符数组前边,后边的字符都是非法字符（此题中），并且有个new_len(所有字符与数字的长度)
- （2）然后按照正常回文串的正常处理
- （3）比较之前要先将大写字母转换成小写字母

```C++
class Solution {
public:
    bool isPalindrome(string s) {
        long long len = s.size();
        if(len == 0)
            return true;
        long long new_len=0;
        for(int i=0;i<len;i++){
            char cur_char = s[i];
            if(isDigit(cur_char) || isLetter(cur_char)){
                // 将字母与数字放到数字前边
                s[new_len] = s[i];
                new_len++;
            }
        }
        for(int i=0;i<new_len/2;i++){//此时 index < new_len的元素都是数字或者字母
            char a = s[i];
            char b = s[new_len - 1 -i];
            // 字母转换小写字母再进行比较
            // 如果是数字，若为相同数字，经过转换他们的字符仍然一致，
            //如果不是相同数字，那么转换出来的字符就会不相同
            if(lowerLetter(a) != lowerLetter(b))
                    return false;
        }
        return true;
    }
    bool isDigit(char c){
        if((c - '0'>=0) && (c - '0') <= 9){ 
            //是数字
            return true;
        }else //不是数字
            return false;
    }
    bool isLetter(char c){
        if(((c - 'a'>=0) && (c - 'a') <= 25) //是小写字母
          ||((c - 'A'>=0) && (c - 'A') <= 25)//是大写字母
          )
        {
            return true;
        }
        else return false;
    }
    char lowerLetter(char c){
       	// c:保证传入的是一个字母，并没有容错机制
        if((c - 'a'>=0) && (c - 'a') <= 25) 
            return c;	// 小写字母直接返回
        else if((c - 'A'>=0) && (c - 'A') <= 25) 
            c = 'a' + (c - 'A');	// 小写字母转换成大写字母
        return c;
    }
};
  
```

##### 收获：

* 大小写字母转换
* 基于ASCII表的数字判断
* 基于ASCII表的字母判断  

### 有效的字母异位词

#### 题目

给定两个字符串 *s* 和 *t* ，编写一个函数来判断 *t* 是否是 *s* 的字母异位词。

**示例 1:**

```
输入: s = "anagram", t = "nagaram"
输出: true
```

**示例 2:**

```
输入: s = "rat", t = "car"
输出: false
```

**说明:**
你可以假设字符串只包含小写字母。

**进阶:**
如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？

##### 解法一：

**解题思路：**

- （1）stimes和titmes分别记录字符串s、字符串t中字母出现的次数
  - stimes[2] = 3： 字符串s中字母c出现了3次
- （2）比较stimes 与titmes的字母出现的次数是否相同

```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        int stimes[26] = {0};
        int ttimes[26] = {0};
        int slen=s.size();
        int tlen=t.size();
        if(slen != tlen) //长度不同直接返回false
            return false;
        for(int i=0;i<slen;i++){
            // 字符串s中,a-z字母所出现的次数
            stimes[s[i] - 'a']++;
            // 字符串t中,a-z字母所出现的次数
            ttimes[t[i] - 'a']++;
        }
        for(int i=0;i<26;i++){
            if(stimes[i] != ttimes[i]) // s中与t中的字母出现的次数不相同，则返回false
                return false;
        }
        return true;
    }
};
```



##### 解法二：

**解题思路：**

- 解法一的改进，将一个数组去掉了，仅保留了一个数组
- 对字符串s，统计为++，对字符串t统计为--，如果最后hash的值全部都是0，则true

```C++
class Solution {
public:
    bool isAnagram(string s, string t) {
        int hash[26] = {0};
        int slen=s.size();
        int tlen=t.size();
        if(slen != tlen) //长度不同直接返回false
            return false;
        for(int i=0;i<slen;i++){
            // 字符串s中,a-z字母所出现的次数++
            hash[s[i] - 'a']++;
            // 字符串t中,a-z字母出现就 次数--
            hash[t[i] - 'a']--;
        }
        for(int i=0;i<26;i++){
            if(hash[i] != 0) 
                return false;// 说明s与t两个字符串的相同字母的个数出现不一致
        }
        return true;
    }
};
```



##### 收获：

* 字母与数组的映射



### 整数反转

#### 题目

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

**示例 1:**

```
输入: 123
输出: 321
```

 **示例 2:**

```
输入: -123
输出: -321
```

**示例 3:**

```
输入: 120
输出: 21
```

**注意:**

假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

##### 解法一：

**解题思路：**

- 扩大法，用一个long保存反转后的数字

```c++
class Solution {
public:
	int reverse(int x) {
		long sum = 0;//一个比int32位数大的数
		while (x != 0) {
			int y = x % 10; 	// 从右到左取出数字
			sum = sum * 10 + y;	//
			if (sum < INT_MIN || sum > INT_MAX) {
                // 反转求和过程中sum溢出int32的数值范围
				return 0;
			}
			x = x / 10;
		}
		return (int) sum;
	}
};
```



##### 解法二：

**解题思路：**

- 关键：

  - ```C++
    //判断数字是否溢出       
    if (tmp / 10 != sum)
          return 0;
    ```

```C++
class Solution {
public:
    int reverse(int x) {
        int sum = 0;
        while (x) {
            int tmp = sum * 10 + x % 10;
            if (tmp / 10 != sum)//溢出后，这里就会不成立了
                return 0;
            sum = tmp;
            x /= 10;
        }
        return sum;
    }
};
```

##### 收获：

* //判断数字是否溢出       

  ```C++
          int tmp = sum * 10 + x % 10;
          if (tmp / 10 != sum)//溢出后，这里就会不成立了
              return 0;
          sum = tmp;
  ```





### 字符串转换整数 (atoi)

#### 题目

请你来实现一个 `atoi` 函数，使其能将字符串转换成整数。

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。

当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。

该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。

注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。

在任何情况下，若函数不能进行有效的转换时，请返回 0。

**说明：**

假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231,  231 − 1]。如果数值超过这个范围，qing返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。

**示例 1:**

```
输入: "42"
输出: 42
```

**示例 2:**

```
输入: "   -42"
输出: -42
解释: 第一个非空白字符为 '-', 它是一个负号。
     我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。
```

**示例 3:**

```
输入: "4193 with words"
输出: 4193
解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。
```

**示例 4:**

```
输入: "words and 987"
输出: 0
解释: 第一个非空字符是 'w', 但它不是数字或正、负号。
     因此无法执行有效的转换。
```

**示例 5:**

```
输入: "-91283472332"
输出: -2147483648
解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 
     因此返回 INT_MIN (−231) 。
```



##### 解法一：

**解题思路：**

- 

```c++
int myAtoi(string str) {
	int len = str.size();
	int i = 0;
	int sign = 1; // 1:正数，-1:负数
	int sum = 0;  // 最终的数字
	while (i < len)
		if (str[i] == ' ')
			i++;
		else
			break;
	if (i == len) // 表示全都是空格
		return 0;
	if (str[i] == '-') {
		sign = -1;
		i++;
		if (i == len)
			return 0; //表示最后一个字符是"-"
	}else if(str[i] == '+'){
        sign = 1;
		i++;
		if (i == len)
			return 0; //表示最后一个字符是"+"
    }
	else if (!isdigit(str[i])) {
		//第一个遇到的字符不是'-',也不是数字
		return 0;
	}
	if (sign == -1 && isdigit(str[i])) {
		//第一个字符遇到的是"-",表示负数 and 当前的 i 指向的字符为数字
		sum = -(str[i] - '0');
		i++;
	}
	while (i < len && isdigit(str[i])) {
		int tmp = sum * 10 + sign*(str[i] - '0');
		if (tmp / 10 != sum) {
			//判断是否溢出
			if (sum > 0)
				return INT_MAX;
			else 
				return INT_MIN;
		}
		sum = tmp;
		i++;
	}
	return sum;
}
```



##### 解法二：

**解题思路：**

- 

```C++

```



##### 收获：

* isdigit函数

* 判断溢出

  * ```C++
            int tmp = sum * 10 + x % 10;
            if (tmp / 10 != sum)//溢出后，这里就会不成立了
                return 0;
            sum = tmp;
    ```

* 对于一个数字 sign = 1 or sign = -1 不要使用 **!sign**，对于bool类型的值才使用**!(非)**

  * ```C++
    	int sign = 1;
      	int signf = -1;
      	cout << sign << " "<< signf <<endl;  // 结果： 1 1
      	cout << !sign << " " << !signf << endl;//结果： 0 0
    ```

* 记得sign 表示正数还是负数，

  * 正数： sum = sum + (str[i] - '0')，

  * 负数：sum = sum - (str[i] - '0')，

  * ```C++
    sum * 10 + sign*(str[i] - '0')
    ```



### 实现strStr()

#### 题目

实现 [strStr()](https://baike.baidu.com/item/strstr/811469) 函数。

给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  **-1**。

**示例 1:**

```
输入: haystack = "hello", needle = "ll"
输出: 2
```

**示例 2:**

```
输入: haystack = "aaaaa", needle = "bba"
输出: -1
```

**说明:**

当 `needle` 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 `needle` 是空字符串时我们应当返回 0 。这与C语言的 [strstr()](https://baike.baidu.com/item/strstr/811469) 以及 Java的 [indexOf()](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#indexOf(java.lang.String)) 定义相符。

#### 题目分析：

1. 

##### 解法一：

O(N*M)

**解题思路：**

1. 

```c++
class Solution {
public:
    int strStr(string haystack, string needle) {
        if(needle == "") return 0;
        int hlen = haystack.size();
        int nlen = needle.size();
        int i=0;
        int j = 0;
        while(hlen - i >= nlen){ // 剩余haystack字符 < needle的字符,这说明不符合匹配
            for(j=0;j<nlen;j++){
                if(haystack[i+j] != needle[j]){
                    // 出现字符不匹配则退出本轮次的循环
                    break;
                }
            }
            if(j == nlen){
                //匹配成功返回下标
                 return i; 
            }
            //从下一个haystack字符开始搜索
            ++i; 
        }
        return -1;
    }
};
```



##### 解法二：

O(N*M)

**解题思路：**

1. 

```C++
class Solution {
public:
    int strStr(string haystack, string needle) {
        int sLen = haystack.size();
        int pLen = needle.size();

        int i = 0;
        int j = 0;
        while (i < sLen && j < pLen)
        {
            if (s[i] == p[j])
            {
                //①如果当前字符匹配成功（即S[i] == P[j]），则i++，j++    
                i++;
                j++;
            }
            else
            {
                //②如果失配（即S[i]! = P[j]），令i = i - (j - 1)，j = 0    
                i = i - j + 1;
                j = 0;
            }
        }
        //匹配成功，返回模式串p在文本串s中的位置，否则返回-1
        if (j == pLen)
            return i - j;
        else
            return -1;
    }
};
```

##### 解法三：

O(N + M)

**解题思路：**

1. 

```C++
class Solution {
public:
    int strStr(string haystack, string needle) {
        int sLen = haystack.size();
        int pLen = needle.size();

        int i = 0;
        int j = 0;
        while (i < sLen && j < pLen)
        {
            if (s[i] == p[j])
            {
                //①如果当前字符匹配成功（即S[i] == P[j]），则i++，j++    
                i++;
                j++;
            }
            else
            {
                //②如果失配（即S[i]! = P[j]），令i = i - (j - 1)，j = 0    
                i = i - j + 1;
                j = 0;
            }
        }
        //匹配成功，返回模式串p在文本串s中的位置，否则返回-1
        if (j == pLen)
            return i - j;
        else
            return -1;
    }
};
```



##### 收获：

### 最长公共前缀

#### 题目

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 `""`。

**示例 1:**

```
输入: ["flower","flow","flight"]
输出: "fl"
```

**示例 2:**

```
输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
```

**说明:**

所有输入只包含小写字母 `a-z` 。

#### 题目分析：

1. 

##### 解法一：

**解题思路：**

1. 找到strs里的字符串的最小长度
2. 纵向比较，比较strs里每个字符串的第0，1,...个字符

```c++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        int min_len = INT_MAX;
        int len = strs.size();
        int tmp_len;
        if(len == 0)
            return "";
        // 找到strs里的字符串的最小长度
        for(int i=0;i<len;i++){
            tmp_len = strs[i].size();
            if(tmp_len < min_len)
                min_len = tmp_len;
        }
        
        string max_str = "";
        int j = 0;
        char cur_char;
        // 纵向比较
        for(int i=0;i<min_len;i++){
            
            for(j=0;j<len;j++){
                cur_char = strs[0][i];
                if(strs[j][i] != cur_char)
                    break;
            }
            if(j == len){
                max_str += cur_char;
            }else 
                break;
        }
        return max_str;
    }
};
```



##### 解法二：

**解题思路：**

1. 纵向比较，比较strs里每个字符串的第0，1,...个字符

```C++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        int len = strs.size();
        if(len == 0) // strs = [],返回""
            return "";
        int slen = strs[0].size();
        string prefix = "";
        for(int i=0;i<slen;i++){
            for(int j=0;j<len;j++){
                if(i >= strs[j].size() || strs[j][i] != strs[0][i]) 
                    return prefix; // i大于当前字符串的长度 或者 字符不等，则返回prefix
            }
            prefix += strs[0][i]; //字符串连接
        }
        return prefix;
    }
};
```



##### 收获：

### 数学

#### 计数质数(素数)

#### 题目

统计所有小于非负整数 *n* 的质数的数量。

**示例:**

```C++
输入: 10
输出: 4
解释: 小于 10 的质数一共有 4 个, 它们是 2, 3, 5, 7 。
```

#### 题目分析：

1. 对于每一个小于n的数都使用素数判断函数判断
   * 素数判断函数：logsqrt(n)
2. O(nlogn)

##### 解法一:（试除法）

```java
class Solution {
    public int countPrimes(int n) {
        int MAX_N = 1500005;
        int count = 0;
        if(n < 3)
            return 0;
        for(int i=2;i < n;i++){ 
			if(isPrime(i))
                count++;
        }
        return count; 
    }
    boolean isPrime(int n)
    {	//判断是否是素数
        
        int i,sqr = sqrt((double)n);// 为什么要开平方请看收获
        for(i=2; i<=sqr; ++i)
        {
            if ( (n%i) == 0 )
                return false;
        }
        return true;
    }
}
```

##### 解法二：（普通素数筛）

**解题思路：**

1. is_prime[]数组，下标为数字，数组中的元素
   * 0：代表是素数
   * 1：表示被筛掉了，不是素数
2. 设素数 i，
   * j = i * 2不是素数
   * j = j + i 也不是素数 （j =  i * 2 + i = i*(2 + 1) 也不是素数）( j = i * 2 + i + i = i*(2 + 1 + 1) 也不是素数),
     * j = i * 2 , j = i * 3,j = i * 4

```c++
class Solution {
    public int countPrimes(int n) {
        int MAX_N = 1500005;
        int count = 0;
        int[] is_prime = new int[MAX_N]; // 0是素数，1表示被筛掉了
        if(n < 3)
            return 0;
        for(int i=2;i < n;i++){ // 使用每个数字来构建筛子
            if(is_prime[i] == 0){
                count++;
                for(int j=i*2;j < n;j=j+i){ //筛子开始筛选，
                    is_prime[j] = 1;
                }
            }
        }
        return count; 
    }
}
```

**问题：**有些数被**重复筛选**了

* 例：6 = 3 * 2 与 6 = 2 * 3 重复筛选了6，还有其它的很多数被重复筛选了

##### 解法三：（线性素数筛）

**解题思路：**

1. prime[]数组，保存素数,按照保存素数的算法，数组里的素数从小到大排列
   - cnt: 到目前为止存的素数个数 + 1
2. 对于每个数，先使用notPrime[i] 判断是不是素数，是就加入 prime[i]
3. **prime[j] * i < n**
   * 因为prime[j] 是素数，i 是可变，prime[j] * i肯定是**非素数**，且prime[j] * i的值**不会重复**

```c++
class Solution {
    public int countPrimes(int n) {
        if (n < 3) return 0;
        // 初始时全都是false，表示全都是素数
        boolean[] notPrime = new boolean[n];
        // 保存素数,按照保存素数的算法，数组里的素数从小到大排列
        int[] prime = new int[n];
        notPrime[0]=notPrime[1]=true;//0和1不是素数
        int cnt = 1;
        for (int i = 2; i < n; i++) {
            if(!notPrime[i])
                prime[cnt++]=i;//是素数
            for(int j=1;j < cnt && prime[j] * i < n;j++)
            {   // cnt:到目前为止存的素数个数 + 1
                // prime[j]: 小于i的素数
                // prime[j] * i: 小于i的素数 与 i相乘
                // 筛选掉非素数
                notPrime[prime[j] * i] = true;//筛掉小于等于i的素数和i的积构成的合数
            }
        }
        return cnt - 1;  
    }
}
```

##### 解法四：

<https://blog.csdn.net/ghsau/article/details/78768157>

**解题思路：**

1. 先筛去偶数(2是素数)
2. 只筛3≤ i < √n 之间的奇数（奇数才有可能是质数）
   * i * i < n 等价于 i < sqrt(n)
   * i = 3,i += 2 查看奇数
3. 对于一个奇数i，**会依次筛掉** i * i, i(i+2), i(i+4), i(i+6)… i(i+2n)   而：(i+2)、(i+4)...(i+2n) 都是奇数
   * 那么**为什么不筛** 3i,5i,7i…(i-4)i,(i-2)i呢, 因为他们**已经被筛**过了，So，接下来对于 i 会依次筛掉i * i, i(i+2), i(i+4), i(i+6)… i(i+2n)
   * 当我们要筛掉奇数i的倍数时,那么i之前的奇数(i-2,i-4…7,5,3)的倍数((i-2)i,(i-4)i…7i,5i,3i)已经被筛掉了
4. 筛掉素数的奇数倍数 j = i * i , j += i * 2
   * j1 = i * i ，j2 = i * i + i * 2 = i * (i + 2), j3 =  i * i + i * 2+ i * 2 = i*(i+4)
     * j1 = i * i,j2 = i * (i + 2), j3 = i*(i+4)      ( i 是奇数， i+2,i+4都是奇数)，所以是筛掉素数的奇数倍数
     * i - 2是上一个奇数 i0
     * 而 第一层循环的 i1、i2是奇数 i2 = i1 + 2
       * j1 = i2*i2 = ( i1 + 2) * ( i1 + 2) ; j2 =  ( i1 + 2) * ( i1 + 4)

```C++
class Solution {
    public int countPrimes(int n) {
        if (n < 3) return 0;
        // 偶数肯定不是素数，所以先除以2表示去除所有偶数的数量
        // 先筛去偶数
        int primeNum = n / 2; 
        // 素数boolean数组，初始化为 false
        // true:表示该非素数被筛选掉了
        // false:表示是素数
        boolean[] isPrime = new boolean[n];
        // 只筛3≤ i < √n 之间的奇数（奇数才有可能是质数）
        for (int i = 3; i * i < n; i += 2) {
            if (!isPrime[i]) {
                // 筛掉素数的奇数倍数
                for (int j = i * i; j < n; j += i * 2) {// 从i * i开始
                    primeNum -= (isPrime[j])? 0: 1;
                    isPrime[j] = true;
                }
            }
        }
        return primeNum;  
    }
}
```



##### 收获：

1. 求素数为什么到平方根就行了?

   * ```c++
     假设数m = p * q，且p ≤ q
     则:m =p * q ≥ p * p,即p ≤ √m
     所以 m 必有一个小于或等于其平方根的因数,且该因数,
     那么验证素数时就只需要验证到其平方根就可以
     ```

     

2. 素数筛法

3. (偶数2是一个素数)大于3的偶数肯定不是素数

4. 只需要求到sqrt(n)就可以了

### 动态规划

#### 设计动态规划的三个步骤

1. 将问题分解成最优子问题；
2. 用递归的方式将问题表述成最优子问题的解；
3. 自底向上的将递归转化成迭代；（递归是自顶向下）;

#### 1、最优子问题

对于连续的 nn 栋房子：H~1~,H~2~,H~3~......H~n~H 1 ,H 2 ,H 3 ......H n ，小偷挑选要偷的房子，且不能偷相邻的两栋房子，方案无非两种：
方案一：挑选的房子中包含最后一栋；
方案二：挑选的房子中不包含最后一栋；
获得的最大收益的最终方案，一定在这两种方案中产生，用代码表述就是：
最优结果 = Math.max(方案一最优结果，方案二最优结果)

#### 2、子问题的递归表述

#### 3、递归转迭代



#### 题目分析：

1. 

##### 解法一：递归解法(超时)

**解题思路：**

1. 递归，会超时，因为很多重复递归解

   climbStairs(5) --->  climbStairs(4) + climbStairs(3)(第一次) 
    climbStairs(4) --->  climbStairs(3)(第二次)  + climbStairs(2)      

   climbStairs(3)出现重复调用

   如果n更大的话就会有更多的重复解出现

```C++
class Solution {
public:
    int climbStairs(int n) {
        if(n < 1)
            return 0;
        else if(n == 1)
            return 1;
        else if(n == 2)
            return 2;
        return climbStairs(n - 1) + climbStairs(n - 2);
    }
};
```

##### 解法二：动态规划(使用数组)

**解题思路：**

1. 使用**数组保存解**,继而一步步往前推,避免了递归出现大量重复解的低效局面

```C++
class Solution {
public:
    int climbStairs(int n) {
        if(n < 1)
            return 0;
        else if(n == 1)
            return 1;
        else if(n == 2)
            return 2;
        int v[n+5] = {0};
        v[0] = 0;
        v[1] = 1;
        v[2] = 2;
        for(int i = 3;i<=n;i++)
            v[i] = v[i - 1] + v[i - 2];
        return v[n];
    }
};

//代码优化
class Solution {
public:
    int climbStairs(int n) {
        int v[n+5] = {0};
        v[0] = 0;  v[1] = 1;  v[2] = 2;
        for(int i = 3;i<=n;i++)
            v[i] = v[i - 1] + v[i - 2];
        return v[n];
    }
};
```



##### 解法三：动态规划（不使用数组）

**解题思路：**

其实可以避免使用数组，因为我们只需要知道到第n阶有多少种方法，只需要知道到n-1阶和到n-2阶有多少种解法，从递推来看就是我们知道了

```
v[1] = 1,v[2] = 2
v[3] = v[1] + v[2]
v[4] = v[3] + v[2]
v[5] = v[4] + v[3]
...
v[n] = v[n-1] + v[n-2]
```

所以我们只需要用两个变量来保存

 * 到第n-1阶有多少种方法
 * 到第n-2阶有多少种方法

```C++
class Solution {
public:
    int climbStairs(int n) {
        if(n < 1)
            return 0;
        else if(n == 1)
            return 1;
        else if(n == 2)
            return 2;
        int first = 1;
        int second = 2;
        int total_n = 0;
        for(int i = 3;i<=n;i++){
            total_n = first + second; // 使用前两阶的结果得到n阶的
            first = second; 	// 变换
            second = total_n;	// 变换
        }
        return total_n;
    }
};
```



