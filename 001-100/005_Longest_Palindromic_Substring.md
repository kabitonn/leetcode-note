# 005. Longest Palindromic Substring(M)
[005. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

## 题目描述(中等)

给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

示例 1：
```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
```

示例 2：
```
输入: "cbbd"
输出: "bb"
```

## 思路

- 暴力法
- 动态规划
- 扩展中心
- 最长公共子串

## 解决方法

### 暴力法

暴力法将选出所有子字符串可能的开始和结束位置，并检验它是不是回文

!> 超时

```java
    public String longestPalindrome(String s) {
        String longestPalindrome = "";
        int len = 0;
        for (int i = 0; i < s.length(); i++) {
            for (int j = i; j < s.length(); j++) {
                String str = s.substring(i, j + 1);
                if (isPalindrome(str) && j + 1 - i > len) {
                    longestPalindrome = str;
                    len = j + 1 - i;
                }
            }
        }
        return longestPalindrome;
    }

    public boolean isPalindrome(String s) {
        int i = 0, j = s.length() - 1;
        while (i < j) {
            if (s.charAt(i++) != s.charAt(j--)) {
                return false;
            }
        }
        return true;
    }
```

时间复杂度：O(n^3)，假设 n 是输入字符串的长度，则 $\binom{n}{2} = \frac{n(n-1)}{2}$，为此类子字符串(不包括字符本身是回文的一般解法)的总数。因为验证每个子字符串需要 O(n) 的时间，所以运行时间复杂度是 O(n^3)

空间复杂度：O(1)。

### 暴力法 优化


P(i,j)
- true  ：子串`s[i,j]`是回文串
- false ：子串`s[i,j]`不是回文串

`P(i,j)=(P(i+1,j−1)&&S[i]==S[j])`

想知道 P（i,j）的情况，不需要调用判断回文串的函数了，只需要知道 P（i + 1，j - 1）的情况就可以了，这样时间复杂度就少了 O(n)。因此可以用动态规划的方法，空间换时间，把已经求出的 P（i，j）存储起来


​	

```java
    public String longestPalindrome1(String s) {
        String longestPalindrome = "";
        int length = s.length();
        boolean[][] P = new boolean[length][length];
        for (int len = 1; len <= length; len++) {
            for (int start = 0; start < length; start++) {
                int end = start + len - 1;
                //下标已经越界，结束本次循环
                if (end >= length) {
                    break;
                }
                //长度为 1 和 2 的单独判断下
                P[start][end] = (len == 1 || len == 2 || P[start + 1][end - 1]) && s.charAt(start) == s.charAt(end);
                if (P[start][end]) {
                    longestPalindrome = s.substring(start, end + 1);
                }
            }
        }
        return longestPalindrome;
    }
```

时间复杂度：O(n^2)。

空间复杂度：O(n^2)，该方法使用 O(n^2)的空间来存储表。

### 动态规划

- dp[i][j]:子串`s[i,j]`是否为回文串
- 状态转移：`dp[i][j]=(dp[i + 1][j - 1]&&S[i]==S[j])`

```java
        public String longestPalindrome2(String s) {
        int n = s.length();
        String longestPalindrome = "";
        boolean[][] dp = new boolean[n][n];
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                //j - i + 1 代表长度
                dp[i][j] = s.charAt(i) == s.charAt(j) && (j - i + 1 <= 2 || dp[i + 1][j - 1]);
                if (dp[i][j] && j - i + 1 > longestPalindrome.length()) {
                    longestPalindrome = s.substring(i, j + 1);
                }
            }
        }
        return longestPalindrome;
    }
```

时间复杂度：O(n^2)。

空间复杂度：O(n^2)，该方法使用 O(n^2)的空间来存储表。


### 动态规划 空间优化

当求第 i 行的时候我们只需要第 i+1 行的信息，并且 j 的话需要 j - 1 的信息，所以和之前一样 j 也需要倒叙。

```java
    public String longestPalindrome3(String s) {
        int n = s.length();
        String longestPalindrome = "";
        boolean[] dp = new boolean[n];
        for (int i = n - 1; i >= 0; i--) {
            for (int j = n - 1; j >= i; j--) {
                dp[j] = s.charAt(i) == s.charAt(j) && (j - i < 2 || dp[j - 1]);
                if (dp[j] && j - i + 1 > longestPalindrome.length()) {
                    longestPalindrome = s.substring(i, j + 1);
                }
            }
        }
        return longestPalindrome;
    }
```


时间复杂度：O(n^2)。

空间复杂度：O(n)，

### 中心扩散法

回文串一定是对称的，所以我们可以每次循环选择一个中心，进行左右扩展，判断左右字符是否相等即可。

![](../assets/leetcode-note/001-100/005-s-5-1.png)

由于存在奇数的字符串和偶数的字符串，所以我们需要从一个字符开始扩展，或者从两个字符之间开始扩展，所以总共有 n+n-1 个中心。

```java
    public String longestPalindrome4(String s) {
        if (s == null || s.length() == 0) {
            return "";
        }
        int maxLen = 0;
        int start = 0, end = 0;
        for (int c = 0; c < s.length(); c++) {
            int len1 = expandAroundCenter(s, c, c);
            int len2 = expandAroundCenter(s, c, c + 1);
            int len = Math.max(len1, len2);
            if (len > maxLen) {
                start = c - (len - 1) / 2;
                end = c + len / 2;
                maxLen = len;
            }
        }
        String longestPalindrome = s.substring(start, end + 1);
        return longestPalindrome;
    }

    private int expandAroundCenter(String s, int left, int right) {
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        }
        return right - left - 1;
    }
```

时间复杂度：O(n^2)，由于围绕中心来扩展回文会耗去 O(n) 的时间，所以总的复杂度为 O(n^2)。

空间复杂度：O(1)

### 最长公共子串

根据回文串的定义，正着和反着读一样，那我们是不是把原来的字符串倒置了，然后找最长的公共子串就可以了。例如 S = "caba" ，S = "abac"，最长公共子串是 "aba"，所以原字符串的最长回文串就是 "aba"。

