<!-- GFM-TOC -->

* [1. 给表达式加括号](#1-给表达式加括号)
* [2. 不同的二叉搜索树](#2-不同的二叉搜索树)
<!-- GFM-TOC -->


# 1. 给表达式加括号

241\. Different Ways to Add Parentheses (Medium)

[Leetcode](https://leetcode.com/problems/different-ways-to-add-parentheses/description/) / [力扣](https://leetcode-cn.com/problems/different-ways-to-add-parentheses/description/)

**加括号体现了优先级计算，因此程序实现计算就行了**

```html
Input: "2-1-1".

((2-1)-1) = 0
(2-(1-1)) = 2

Output : [0, 2]
```

```java
public List<Integer> diffWaysToCompute(String input) {
    List<Integer> ways = new ArrayList<>();
    for (int i = 0; i < input.length(); i++) {
        char c = input.charAt(i);
        if (c == '+' || c == '-' || c == '*') {
            //计算左子树，左子树有不同的计算方式(括号加在不同位置)，因此返回一个序列
            List<Integer> left = diffWaysToCompute(input.substring(0, i)); 
            //计算右子树，右子树有不同的计算方式(括号加在不同位置)，因此返回一个序列
            List<Integer> right = diffWaysToCompute(input.substring(i + 1)); 
            // 处理所有运算结果
            for (int l : left) {
                for (int r : right) {
                    switch (c) {
                        case '+':
                            ways.add(l + r);
                            break;
                        case '-':
                            ways.add(l - r);
                            break;
                        case '*':
                            ways.add(l * r);
                            break;
                    }
                }
            }
        }
    }
    if (ways.size() == 0) {
        //说明直接返回一个数字(一位或者多位的都可以处理)
        ways.add(Integer.valueOf(input));
    }
    return ways; // 返回不同计算方式产生的结果序列
}
```

# 2. 不同的二叉搜索树

95\. Unique Binary Search Trees II (Medium)

[Leetcode](https://leetcode.com/problems/unique-binary-search-trees-ii/description/) / [力扣](https://leetcode-cn.com/problems/unique-binary-search-trees-ii/description/)

给定一个数字 n，要求生成所有值为 1...n 的二叉搜索树。

```html
Input: 3
Output:
[
  [1,null,3,2],
  [3,2,null,1],
  [3,1,null,null,2],
  [2,1,3],
  [1,null,2,null,3]
]
Explanation:
The above output corresponds to the 5 unique BST's shown below:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

```java
public List<TreeNode> generateTrees(int n) {
    // 返回的所有不同树的根节点
    if (n < 1) {
        return new LinkedList<TreeNode>();
    }
    return generateSubtrees(1, n);
}

private List<TreeNode> generateSubtrees(int s, int e) {
    // 生成子树
    List<TreeNode> res = new LinkedList<TreeNode>(); //子树序列
    if (s > e) {
        // 返回一个空节点序列
        res.add(null);
        return res;
    }
    for (int i = s; i <= e; ++i) { // 初始根节点i
        // 根、左、右遍历
        // 所有可能的左子树，如果选择了一个i作为根节点
        List<TreeNode> leftSubtrees = generateSubtrees(s, i - 1);
        // 所有可能的右子树，如果选择了一个i作为根节点
        List<TreeNode> rightSubtrees = generateSubtrees(i + 1, e);
        // 连接左和右子树到根节点i
        for (TreeNode left : leftSubtrees) {		// 左子树的根节点序列
            for (TreeNode right : rightSubtrees) { 	// 右子树的根节点序列
                TreeNode root = new TreeNode(i);// 根
                // 左、右子树的根节点连接到根上
                root.left = left;
                root.right = right;
                res.add(root); // 由于循环，加入root到res时，它的左右子节点都可能不同
            }
        }
    }
    return res; // 返回的所有不同树的根节点
}
```






<div align="center"><img width="320px" src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/githubio/公众号二维码-2.png"></img></div>
