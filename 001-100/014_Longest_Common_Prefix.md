# 014. Longest Common Prefix(E)
[014. Longest Common Prefix](https://leetcode-cn.com/problems/longest-common-prefix/)

## 题目描述\(简单\)

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string "".

Example 1:

```
Input: ["flower","flow","flight"]
Output: "fl"
```

Example 2:

```
Input: ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

**Note:**

> All given inputs are in lowercase letters a-z

## 思路

1. 对所有字符串从第一列开始比较
2. 前一个依次和后一个求LCP，
3. 递归，两个两个分别求LCP

## 解决方法

### 垂直比较

![](../assets/leetcode-note/001-100/014-s-1.png)
所有字符串垂直排列，然后一列一列的比较，直到某一个字符串到达结尾或者该列字符不完全相同。
首先找到最短字符串长度，然后一列一列比较
```java
    public String longestCommonPrefix(String[] strs) {
        if(strs == null ||strs.length == 0)
            return "";
        int min = Integer.MAX_VALUE;
        for (String str : strs)
            min = Math.min(min, str.length());
        char c;
        int i=0;
        for(;i<min;i++) {
            c = strs[0].charAt(i);
            for(int j=1;j<strs.length;j++) {
                if(strs[j].charAt(i)!=c) {
                    return strs[0].substring(0, i);
                }
            }
        }
        return strs[0].substring(0, i);
    }
```
官方的代码如下
```java
    public String longestCommonPrefix3(String[] strs) {
        if(strs == null ||strs.length == 0)
            return "";
        char c;
        for(int i=0;i<strs[0].length();i++) {
            c = strs[0].charAt(i);
            for(int j=1;j<strs.length;j++) {
                if(strs[j].length()<=i||strs[j].charAt(i)!=c) {
                    return strs[0].substring(0, i);
                }
            }
        }
        return strs[0];
    }
```

时间复杂度：最坏的情况就是 n 个 长度为 m 的完全一样的字符串，假设 S 是所有字符的和，那么 S = m \* n，时间复杂度就是 O(S)。当然正常情况下并不需要比较所有字符串，最多比较 n * minLen 个字符就可以了。

空间复杂度：O(1)，常数个额外空间。

### 水平比较

![](../assets/leetcode-note/001-100/014-s-2.png)

```java
    public String longestCommonPrefix2(String[] strs) {
        if(strs == null ||strs.length == 0)
            return "";
        String prefix = strs[0];
        for(int i=1;i<strs.length;i++) {
            while(strs[i].indexOf(prefix)!=0) {
                prefix=prefix.substring(0, prefix.length()-1);
                if(prefix.length()==0)
                    return "";
            }
        }
        return prefix;
    }
```
时间复杂度：最坏情况和解法一是一样，n 个长度为 m 的完全相同的字符，就要比较所有的字符 S，S = n * m 。但对于正常情况，处于最短字符串前的字符串依旧要比较所有字符，而不是最短字符串个字符，相对于解法一较差。

空间复杂度：O(1)。

### 递归分治

![](../assets/leetcode-note/001-100/014-s-3.png)

把原来的数组分成两部分，求出左半部分的最长公共前缀，求出右半部分的最长公共前缀，然后求出的两个结果再求最长公共前缀，就是最后的结果了。

求左半部分的最长公共前缀，我们可以继续把它分成两部分，按照上边的思路接着求。然后一直分成两部分，递归下去。

直到该部分只有 1 个字符串，那么最长公共子串就是它本身了，直接返回就可以了。

```java
    public String longestCommonPrefix(String[] strs) {
        if(strs == null ||strs.length == 0)
            return "";
        return longestCommonPrefix(strs,0,strs.length-1);
    }

    public String longestCommonPrefix(String[] strs, int left, int right) {
        if(left == right)
            return strs[left];
        int mid = (left+right)/2;
        String lcpleft = longestCommonPrefix(strs, left, mid);
        String lcpright = longestCommonPrefix(strs, mid+1, right);
        String lcp = commonPrefix(lcpleft,  lcpright);
        return lcp;
    }

    public String commonPrefix(String leftStr, String rightStr) {
        int min = Math.min(leftStr.length(), rightStr.length());
        for(int i=0;i<min;i++) {
            if(leftStr.charAt(i)!=rightStr.charAt(i)) {
                return leftStr.substring(0, i);
            }
        }
        return leftStr.substring(0,min);
    }
```



