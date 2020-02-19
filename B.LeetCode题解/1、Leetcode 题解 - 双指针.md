<!-- GFM-TOC -->

* [1. 有序数组的 Two Sum](#1-有序数组的-two-sum)
* [2. 两数平方和](#2-两数平方和)
* [3. 反转字符串中的元音字符](#3-反转字符串中的元音字符)
* [4. 回文字符串](#4-回文字符串)
* [5. 归并两个有序数组](#5-归并两个有序数组)
* [6. 判断链表是否存在环](#6-判断链表是否存在环)
* [7. 最长子序列](#7-最长子序列)
<!-- GFM-TOC -->


双指针主要用于遍历数组，两个指针指向不同的元素，从而协同完成任务。

# 4. 回文字符串

680\. Valid Palindrome II (Easy)

[Leetcode](https://leetcode.com/problems/valid-palindrome-ii/description/) / [力扣](https://leetcode-cn.com/problems/valid-palindrome-ii/description/)

```html
Input: "abca"
Output: True
Explanation: You could delete the character 'c'.
```

题目描述：可以删除一个字符，判断是否能构成回文字符串。

所谓的回文字符串，是指具有左右对称特点的字符串，例如 "abcba" 就是一个回文字符串。

使用双指针可以很容易判断一个字符串是否是回文字符串：令一个指针从左到右遍历，一个指针从右到左遍历，这两个指针同时移动一个位置，每次都判断两个指针指向的字符是否相同，如果都相同，字符串才是具有左右对称性质的回文字符串。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/fcc941ec-134b-4dcd-bc86-1702fd305300.gif" width="250px"> </div><br>
本题的关键是处理删除一个字符。在使用双指针遍历字符串时，如果出现两个指针指向的字符不相等的情况，我们就试着删除一个字符，再判断删除完之后的字符串是否是回文字符串。

在判断是否为回文字符串时，我们不需要判断整个字符串，因为左指针左边和右指针右边的字符之前已经判断过具有对称性质，所以只需要判断中间的子字符串即可。

**在试着删除字符时，我们既可以删除左指针指向的字符，也可以删除右指针指向的字符。**

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/db5f30a7-8bfa-4ecc-ab5d-747c77818964.gif" width="300px"> </div><br>
```java
public boolean validPalindrome(String s) {
    for (int i = 0, j = s.length() - 1; i < j; i++, j--) {
        if (s.charAt(i) != s.charAt(j)) {
            // 此处代码很关键，要判断两种可能，试着删除j指向的或者试着删除i指向
            return isPalindrome(s, i, j - 1) || isPalindrome(s, i + 1, j);
        }
    }
    return true;
}

private boolean isPalindrome(String s, int i, int j) {
    while (i < j) {
        if (s.charAt(i++) != s.charAt(j--)) {
            return false;
        }
    }
    return true;
}
```



# 7. 最长子序列

524\. Longest Word in Dictionary through Deleting (Medium)

[Leetcode](https://leetcode.com/problems/longest-word-in-dictionary-through-deleting/description/) / [力扣](https://leetcode-cn.com/problems/longest-word-in-dictionary-through-deleting/description/)

```
Input:
s = "abpcplea", d = ["ale","apple","monkey","plea"]

Output:
"apple"
```

题目描述：删除 s 中的一些字符，使得它构成字符串列表 d 中的一个字符串，找出能构成的最长字符串。如果有多个相同长度的结果，返回字典序的最小字符串。

通过删除字符串 s 中的一个字符能得到字符串 t，可以认为 t 是 s 的子序列，我们可以使用双指针来判断一个字符串是否为另一个字符串的子序列。

```java
public String findLongestWord(String s, List<String> d) {
    String longestWord = "";
    for (String target : d) {
        int l1 = longestWord.length(), l2 = target.length();
        if (l1 > l2 || (l1 == l2 && longestWord.compareTo(target) < 0)) {
            // 比较当前最长子串和带比较target串的长度  或者 相等长度时的字典序
            continue;
        }
        if (isSubstr(s, target)) {
            longestWord = target;
        }
    }
    return longestWord;
}

private boolean isSubstr(String s, String target) {
    // 是否为子串的判断
    int i = 0, j = 0;
    while (i < s.length() && j < target.length()) {
        if (s.charAt(i) == target.charAt(j)) {
            // 相等的情况j++指针向后
            j++;
        }
        //如果本次字符不匹配，j不变，i++(等价于删除s中i指向的字符)
        i++;
    }
    return j == target.length();
}
```






<div align="center"><img width="320px" src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/githubio/公众号二维码-2.png"></img></div>
