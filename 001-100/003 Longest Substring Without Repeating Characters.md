# 003. Longest Substring Without Repeating Characters\(M\)

[003. Longest Substring Without Repeating Characters](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

## 题目描述\(中等\)

Given a string, find the length of the longest substring without repeating characters.

Example 1:

```
Input: "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

Example 2:

```
Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

Example 3:

```
Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
```

**Note**

> that the answer must be a substring, "pwke" is a subsequence and not a substring.

## 思路

1. 遍历所有情况，求最大值
2. 双指针，若出现重复，去除当前子串头部元素，继续遍历
3. 滑动窗口，若出现重复，找出子串内重复的元素j'，直接令窗口头向后滑动为j'+1
4. 数组模拟map实现滑动窗口，适用key数目较少情况 

## 解决方法

### 1. 暴力法

依次判断以当前元素为首的无重复元素子串是否为最长子串，以i为首j为尾的子串是否为无重复元素子串，若为重复元素子串，循环i+1为首的子串；若为无重复元素子串，判断当前长度j-i+1与maxNum并修改。

```java
    public int lengthOfLongestSubstring(String s) {
        if(s.length()==0) {
            return 0;
        }
        int maxNum = 1;
        char[] str = s.toCharArray();
        for(int i=0;i<str.length;i++) {
            p:for(int j=i+1;j<str.length;j++) {
                for(int k=0;k<j-i;k++) {
                    if(str[j]==str[i+k]) {
                        break p;
                    }
                }
                if(j-i+1>maxNum) {
                    maxNum = j-i+1;
                }
            }
        }
        return maxNum;
    }
```

时间复杂度$$ O(n^3) $$

```java
    public int lengthOfLongestSubstring(String s) {
        int maxNum = 0;
        Set<Character> set = new HashSet<>();
        char[] str = s.toCharArray();
        int n = str.length;
        for(int i=0;i<n;i++) {
            set.add(str[i]);
            for(int j=i+1;j<n;j++) {
                if(!set.contains(str[j])) {
                    set.add(str[j]);
                }
                else {
                    break;
                }
            }
            maxNum = Math.max(maxNum,set.size());
            set.clear();
        }
```

时间复杂度O\(n^2\)  
空间复杂度O\(min\(n,m\)\),需要 O\(k\) 的空间来检查子字符串中是否有重复字符，其中 k 表示 Set 的大小。而 Set 的大小取决于字符串n 的大小以及字符集字母m的大小。

### 2. 双指针\(滑动窗口\)

通过使用 HashSet 作为滑动窗口，我们可以用 O\(1\) 的时间来完成对字符是否在当前的子字符串中的检查。

滑动窗口是数组/字符串问题中常用的抽象概念。 窗口通常是在数组/字符串中由开始和结束索引定义的一系列元素的集合，即 \[i, j\)\(左闭，右开\)。而滑动窗口是可以将两个边界向某一方向“滑动”的窗口。例如，我们将\[i,j\)向右滑动1个元素，则它将变为 \[i+1, j+1\)（左闭，右开）。

回到我们的问题，我们使用 HashSet 将字符存储在当前窗口\[i, j\)\(最初j = i\)中。 然后我们向右侧滑动索引j，如果它不在 HashSet 中，我们会继续滑动 j。直到 s\[j\] 已经存在于 HashSet 中。此时，我们找到的没有重复字符的最长子字符串将会以索引i开头。如果我们对所有的i这样做，就可以得到答案。

![](/assets/001-100/003-solution-2-1.png)

```java
    public int lengthOfLongestSubstring03(String s) {
        int n = s.length();
        Set<Character> set = new HashSet<>();
        int maxNum = 0, i = 0, j = 0;
        while (i < n && j < n) {
            // try to extend the range [i, j]
            if (!set.contains(s.charAt(j))){
                set.add(s.charAt(j++));
                maxNum = Math.max(maxNum, j - i);
            }
            else {
                set.remove(s.charAt(i++));
            }
        }
        return maxNum;
    }
```

时间复杂度：O\(2n\) = O\(n\)，在最糟糕的情况下，每个字符将被i和j访问两次。  
空间复杂度：O\(min\(m, n\)\)，与之前的方法相同。滑动窗口法需要 O\(k\)的空间，其中k 表示 Set 的大小。而 Set 的大小取决于字符串 n的大小以及字符集 / 字母m的大小。

### 3. 优化滑动窗口

![](/assets/001-100/003-solution-3-1.png)

![](/assets/001-100/003-solution-3--2.png)

上述的方法最多需要执行 2n 个步骤。事实上，它可以被进一步优化为仅需要 n 个步骤。我们可以定义字符到索引的映射，而不是使用集合来判断一个字符是否存在。 当我们找到重复的字符时，我们可以立即跳过该窗口。

也就是说，如果 s\[j\] 在 \[i,j\) 范围内有与 j'重复的字符，我们不需要逐渐增加 i 。 我们可以直接跳过 \[i,j'\] 范围内的所有元素，并将 i 变为 j' + 1

```java
    public int lengthOfLongestSubstring4(String s) {
        int n = s.length();
        int maxNum = 0;
        Map<Character, Integer> map = new HashMap<>();
        for(int i=0,j=0;j<n;j++) {
            char c = s.charAt(j);
            if(map.containsKey(c)) {
                i = Math.max(map.get(c),i);
            }
            maxNum = Math.max(maxNum, j-i+1);
            map.put(c, j+1);    //i=j'+1
        }

        return maxNum;
    }
```

与上述解法相比，由于采取了 i 跳跃的形式，所以 map 之前存的字符没有进行 remove ，所以 if 语句中进行了Math.max \( map.get \(c\), i \)，要确认得到的下标不是 i 前边的。

在每次循环中都进行更新，因为 maxNum 更新前 i 都进行了更新，已经保证了当前的子串符合条件，所以可以更新 maxNum 。而解法二中，只有当当前的子串不包含当前的字符时，才进行更新。

时间复杂度：我们将 2n 优化到了 n ，但最终还是和之前一样，O\(n\)。

空间复杂度：也是一样的，O\(min\(m, n\)\)。

### 4. 优化滑动窗口改

采用数组模拟map，适用于字符集较小情况。

```java
    public int lengthOfLongestSubstring5(String s) {
        int n = s.length(), maxNum = 0;
        int[] map = new int[128];
        for (int j = 0, i = 0; j < n; j++) {
            i = Math.max(map[s.charAt(j)], i);
            maxNum = Math.max(maxNum, j - i + 1);
            map[s.charAt(j)] = j + 1;    //i=j'+1
        }
        return maxNum;
    }
```

时间复杂度：O\(n\)

空间复杂度：O\(m\)，m代表字符集的大小。

