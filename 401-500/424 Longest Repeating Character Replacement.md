
# 424. Longest Repeating Character Replacement(M)

[424. 替换后的最长重复字符](https://leetcode-cn.com/problems/longest-repeating-character-replacement/)

## 题目描述(中等)

给你一个仅由大写英文字母组成的字符串，你可以将任意位置上的字符替换成另外的字符，总共可最多替换 `k` 次。在执行上述操作后，找到包含重复字母的最长子串的长度。

**注意**:
字符串长度 和 k 不会超过 10^4。

示例 1:
```
输入:
s = "ABAB", k = 2
输出:
4
解释:
用两个'A'替换为两个'B',反之亦然。
```

示例 2:
```
输入:
s = "AABABBA", k = 1
输出:
4
解释:
将中间的一个'A'替换为'B',字符串变为 "AABBBBA"。
子串 "BBBB" 有最长重复字母, 答案为 4。
```

## 思路

- 暴力遍历
- 滑动窗口

## 解决方法

### 暴力遍历

遍历以每一个字符作为首字符的字符串所有情况
- 一次遍历过程不同字符需要替换，遍历后若仍可替换字符，可首字符前的字符替换

```java
    public int characterReplacement(String s, int k) {
        int max = 0;
        char[] chars = s.toCharArray();
        int n = s.length();
        for (int i = 0; i < n - max; i++) {
            int replaceNum = 0, len = 1;
            for (int j = i + 1; j < n; j++) {
                if (chars[j] == chars[i]) {
                    len++;
                } else if (replaceNum < k) {
                    replaceNum++;
                    len++;
                } else {
                    break;
                }
            }
            if (replaceNum < k) {
                len += i > k - replaceNum ? k - replaceNum : i;
            }
            max = Math.max(max, len);
        }
        return max;
    }
```

时间复杂度：O(n^2)

### 滑动窗口

维护一个窗口[i,j],窗口内字符都可以为s[i]
- 窗口移动过程中，若不能继续替换字符，窗口右移，i直接取后序第一个不为s[i]的索引
- 遍历到最后[i,n)的状态，若仍可替换字符，可将i之前的字符替换为s[i]，得到一个新的字符串

```java
    public int characterReplacement1(String s, int k) {
        int max = 0;
        int i = 0, j = 1;
        int n = s.length();
        int replaceNum = 0, replaceIndex = -1;
        while (j < n) {
            if (s.charAt(j) == s.charAt(i)) {
                j++;
            } else if (replaceNum < k) {
                if (replaceNum == 0) {
                    replaceIndex = j;
                }
                replaceNum++;
                j++;
            } else {
                max = Math.max(max, j - i);
                replaceNum = 0;
                i = replaceIndex != -1 ? replaceIndex : j;
                j = i + 1;
            }
        }
        if (replaceNum < k) {
            if (i > k - replaceNum) {
                i -= k - replaceNum;
            } else {
                i = 0;
            }
        }
        max = Math.max(max, n - i);
        return max;
    }

```

### 滑动窗口

字符表来表示窗口，窗口的大小与最多字符个数之间的差值表示需要被替换的字符个数，当需要替换的字符个数大于k时，我们需要缩小窗口，也就是left右移，直到需要替换的字符个数等于k时，可以得到当前字符串长度


```java
    public int characterReplacement2(String s, int k) {
        int max = 0;
        int[] count = new int[26];
        int n = s.length();
        char[] chars = s.toCharArray();
        int i = 0, maxCount = 0;
        for (int j = 0; j < n; j++) {
            int index = chars[j] - 'A';
            count[index]++;
            maxCount = Math.max(maxCount, count[index]);
            if (j - i + 1 - maxCount > k) {
                count[chars[i++] - 'A']--;
            }
            max = Math.max(max, j - i + 1);
        }
        return max;
    }
```