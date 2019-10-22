# 139. Word Break(M)


[139. 单词拆分](https://leetcode-cn.com/problems/word-break/)


## 题目描述(中等)

Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, determine if s can be segmented into a space-separated sequence of one or more dictionary words.

**Note**:

- The same word in the dictionary may be reused multiple times in the segmentation.
- You may assume the dictionary does not contain duplicate words.

Example 1:
```
Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```
Example 2:
```
Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
             Note that you are allowed to reuse a dictionary word.
```
Example 3:
```
Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false
```

## 思路

- 回溯

## 解决方法



### 递归回溯 记忆化

HashMap存储考虑过的解，key 的话就存 temp，value 的话就代表以当前 temp 开始的字符串，经过后边的尝试是否能达到目标字符串 s

```java
    public boolean wordBreak(String s, List<String> wordDict) {
        Map<String, Boolean> map = new HashMap<>();
        return wordBreak(s, wordDict, "", map);
    }

    public boolean wordBreak(String s, List<String> wordList, String temp, Map<String, Boolean> map) {
        if (temp.length() > s.length()) {
            return false;
        }
        if (map.containsKey(temp)) {
            return map.get(temp);
        }
        for (int i = 0; i < temp.length(); i++) {
            if (temp.charAt(i) != s.charAt(i)) {
                return false;
            }
        }
        if (temp.length() == s.length()) {
            return true;
        }
        for (String word : wordList) {
            if (wordBreak(s, wordList, temp + word, map)) {
                map.put(temp, true);
                return true;
            }
        }
        map.put(temp, false);
        return false;
    }
```

### 递归分治 记忆化

dp[i,j)，表示从 s 的第 i 个字符开始，到第 j 个字符的前一个结束的字符串是否能由 wordDict 构成。


```
dp[0,len) =    wordDict.contains(s[i,len)) && dp[0,1)
            || wordDict.contains(s[2,len)) && dp[0,2)
            || wordDict.contains(s[3,len)) && dp[0,3)   
            ...
            || wordDict.contains(s[len - 1,len)) && dp[0,len - 1)

```
dp[0,len) 就代表着 s 是否能由 wordDict 构成。

```java
    public boolean wordBreak1(String s, List<String> wordDict) {
        Set<String> wordSet = new HashSet<>(wordDict);
        return wordBreak1(s, wordSet, new HashMap<String, Boolean>());
    }

    public boolean wordBreak1(String s, Set<String> wordSet, Map<String, Boolean> map) {
        if (s.length() == 0) {
            return true;
        }
        if (map.containsKey(s)) {
            return map.get(s);
        }
        for (int i = 0; i < s.length(); i++) {
            if (wordSet.contains(s.substring(i)) && wordBreak1(s.substring(0, i), wordSet, map)) {
                map.put(s, true);
                return true;
            }
        }
        
        map.put(s, false);
        return false;
    }
```

另一种分治思路

```
dp[0,len) =    wordDict.contains(s[0,1)) && dp[1,len)
            || wordDict.contains(s[0,2)) && dp[2,len)
            || wordDict.contains(s[0,3)) && dp[0,len)
            ...
            || wordDict.contains(s[0,len)) && dp[len,len)

```




```java
    public boolean wordBreak1(String s, List<String> wordDict) {
        Set<String> wordSet = new HashSet<>(wordDict);
        return wordBreak1(s, wordSet, new HashMap<String, Boolean>());
    }

    public boolean wordBreak1(String s, Set<String> wordSet, Map<String, Boolean> map) {
        if (s.length() == 0) {
            return true;
        }
        if (map.containsKey(s)) {
            return map.get(s);
        }
        for (int i = 1; i <= s.length(); i++) {
            if (wordSet.contains(s.substring(0, i)) && wordBreak1(s.substring(i), wordSet, map)) {
                map.put(s, true);
                return true;
            }
        }
        map.put(s, false);
        return false;
    }

```

