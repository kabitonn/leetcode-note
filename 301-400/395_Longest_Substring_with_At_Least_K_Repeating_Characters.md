# 395. Longest Substring with At Least K Repeating Characters(M)

[395. 至少有K个重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-with-at-least-k-repeating-characters/)

## 题目描述(中等)

找到给定字符串（由小写字符组成）中的最长子串 T ， 要求 T 中的每一字符出现次数都不少于 k 。输出 T 的长度。

示例 1:
```
输入:
s = "aaabb", k = 3

输出:
3

最长子串为 "aaa" ，其中 'a' 重复了 3 次。
```
示例 2:
```
输入:
s = "ababbc", k = 2

输出:
5

最长子串为 "ababb" ，其中 'a' 重复了 2 次， 'b' 重复了 3 次。
```
## 思路

- 遍历子串
- 滑动窗口
- 

## 解决方法

### 暴力法

```java
    public int longestSubstring0(String s, int k) {
        int max = 0;
        int n = s.length();
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j <= n; j++) {
                if (valid(s.substring(i, j), k)) {
                    max = Math.max(max, j - i);
                }
            }
        }
        return max;
    }

    public boolean valid(String s, int k) {
        int[] nums = new int[26];
        for (char c : s.toCharArray()) {
            nums[c - 'a']++;
        }
        for (int num : nums) {
            if (num != 0 && num < k) {
                return false;
            }
        }
        return true;
    }
```

### 滑动窗口

从 len=n 递减判断 len是否为符合要求子串
- 判断长度为len的[i,j]的窗口内重复字符个数是否符合

```java
    public int longestSubstring(String s, int k) {
        if (k < 2) {
            return s.length();
        }
        int n = s.length();
        char[] chs = s.toCharArray();
        for (int len = n; len >= k; len--) {
            int[] nums = new int[26];
            int i = 0, j = 0;
            while (j <= n && i <= n - len) {
                int l = j - i;
                if (l < len) {
                    nums[chs[j++] - 'a']++;
                } else if (l == len) {
                    boolean flag = true;
                    for (int num : nums) {
                        if (num != 0 && num < k) {
                            flag = false;
                            break;
                        }
                    }
                    if (flag) {
                        return len;
                    }
                    nums[chs[i++] - 'a']--;
                }
            }
        }
        return 0;
    }
```

### 递归 两端向内收缩

递归拆分子串，分治。先统计出每个字符出现的频次，维护一对双指针，从首尾开始统计，从首尾往中间排除，如果出现次数小于k则不可能出现在最终子串中，排除并挪动指针，然后得到临时子串，依次从头遍历，一旦发现出现频次小于k的字符，以该字符为分割线，分别递归求其最大值返回


```java
    public int longestSubstring1(String s, int k) {
        if (k < 2) {
            return s.length();
        }
        int n = s.length();
        if (k > n) {
            return 0;
        }
        return count(s.toCharArray(), 0, n - 1, k);
    }

    public int count(char[] chars, int left, int right, int k) {
        int[] nums = new int[26];
        for (int i = left; i <= right; i++) {
            nums[chars[i] - 'a']++;
        }
        while (right - left + 1 >= k && nums[chars[left] - 'a'] < k) {
            left++;
        }
        while (right - left + 1 >= k && nums[chars[right] - 'a'] < k) {
            right--;
        }
        if (right - left + 1 < k) {
            return 0;
        }
        int i = left;
        for (; i <= right; i++) {
            if (nums[chars[i] - 'a'] < k) {
                break;
            }
        }
        if (i <= right) {
            return Math.max(count(chars, left, i - 1, k), count(chars, i + 1, right, k));
        }
        return right - left + 1;
    }
```