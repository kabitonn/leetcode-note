
# 567. Permutation in String(M)
 
[567. 字符串的排列](https://leetcode-cn.com/problems/permutation-in-string/)

## 题目描述(中等)

给定两个字符串 s1 和 s2，写一个函数来判断 s2 是否包含 s1 的排列。

换句话说，第一个字符串的排列之一是第二个字符串的子串。

示例1:
```
输入: s1 = "ab" s2 = "eidbaooo"
输出: True
解释: s2 包含 s1 的排列之一 ("ba").
```

示例2:
```
输入: s1= "ab" s2 = "eidboaoo"
输出: False
``` 

**注意**：
- 输入的字符串只包含小写字母
- 两个字符串的长度都在 [1, 10,000] 之间


## 思路

- 排列后判断
- 等长异位词判断

## 解决方法

### 暴力 排列后判断

!> 超时

```java
    public boolean checkInclusion(String s1, String s2) {
        char[] chars1 = s1.toCharArray();
        return permutation(chars1, 0, s2);
    }

    public boolean permutation(char[] chars, int pos, String s) {
        if (pos == chars.length) {
            return s.contains(new String(chars));
        }
        for (int i = pos; i < chars.length; i++) {
            swap(chars, i, pos);
            if (permutation(chars, pos + 1, s)) {
                return true;
            }
            swap(chars, i, pos);
        }
        return false;
    }
    public void swap(char[] chars, int i, int j) {
        char c = chars[i];
        chars[i] = chars[j];
        chars[j] = c;
    }
```

### 排序字符串判断异位词

遍历等长字符串 排序后比较相等判断是否为异位词

```java
    public boolean checkInclusion2(String s1, String s2) {
        s1 = sort(s1);
        int n1 = s1.length(), n2 = s2.length();
        for (int i = 0; i <= n2 - n1; i++) {
            String s = s2.substring(i, i + n1);
            s = sort(s);
            if (s1.equals(s)) {
                return true;
            }
        }
        return false;
    }

    public String sort(String s) {
        char[] chars = s.toCharArray();
        Arrays.sort(chars);
        return new String(chars);
    }
```

### 滑动窗口 比较数组 判断异位词

取和匹配字符串相同长度的窗口，统计各字符频率，和匹配字符串字符频率比较是否相等判断是否为异位词

```java
    public boolean checkInclusion3(String s1, String s2) {
        int n1 = s1.length(), n2 = s2.length();
        if (n1 > n2) {
            return false;
        }
        int[] map = new int[26];
        for (char ch : s1.toCharArray()) {
            map[ch - 'a']++;
        }
        int[] substr = new int[26];
        for (int i = 0; i < n1; i++) {
            substr[s2.charAt(i) - 'a']++;
        }
        if (Arrays.equals(map, substr)) {
            return true;
        }
        for (int i = n1; i < n2; i++) {
            substr[s2.charAt(i) - 'a']++;
            substr[s2.charAt(i - n1) - 'a']--;
            if (Arrays.equals(map, substr)) {
                return true;
            }
        }
        return false;
    }
```

### 滑动窗口 统计满足数目 判断异位词

count 记录满足匹配字符串需要的字符个数，若窗口内的字符是需要的，则count+1,同时需求数-1

```java
    public boolean checkInclusion4(String s1, String s2) {
        int n1 = s1.length(), n2 = s2.length();
        if (n1 > n2) {
            return false;
        }
        int[] need = new int[26];
        for (char ch : s1.toCharArray()) {
            need[ch - 'a']++;
        }
        int count = 0;
        for (int i = 0; i < n1; i++) {
            if (need[s2.charAt(i) - 'a']-- > 0) {
                count++;
            }
        }
        if (count == n1) {
            return true;
        }
        for (int i = n1; i < n2; i++) {
            if (need[s2.charAt(i) - 'a']-- > 0) {
                count++;
            }
            if (need[s2.charAt(i - n1) - 'a']++ >= 0) {
                count--;
            }
            if (count == n1) {
                return true;
            }
        }
        return false;
    }
    public boolean checkInclusion1(String s1, String s2) {
        int n1 = s1.length(), n2 = s2.length();
        int[] need = new int[26];
        for (char ch : s1.toCharArray()) {
            need[ch - 'a']++;
        }
        int i = 0, j = 0;
        int count = 0;
        while (j < n2) {
            if (need[s2.charAt(j++) - 'a']-- > 0) {
                count++;
            }
            if (count == n1) {
                return true;
            }
            if (j - i == n1) {
                if (need[s2.charAt(i++) - 'a']++ >= 0) {
                    count--;
                }
            }
        }
        return false;
    }
```