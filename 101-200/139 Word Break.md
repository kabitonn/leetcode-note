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
- 分治
- 动态规划

## 解决方法



### 递归回溯 记忆化

HashMap存储考虑过的解，value 的话就代表以当前 key开始的字符串，经过后边的尝试是否能达到目标字符串 s


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

HashMap存储考虑过的解，value 的话就代表以当前 key开始的字符串，经过后边的尝试是否能达到目标字符串 s



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

HashMap存储考虑过的解，value 的话就代表以当前 key作为结束的字符串，经过后边的尝试是否能达到目标字符串 s


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
        for (int i = len; i>0; i--) {
            if (wordSet.contains(s.substring(0, i)) && wordBreak1(s.substring(i), wordSet, map)) {
                map.put(s, true);
                return true;
            }
        }
        map.put(s, false);
        return false;
    }
```

分治思路二的另一种写法

```java
    public boolean wordBreak3(String s, List<String> wordDict) {
        return wordBreak3(s, new HashSet(wordDict), 0);
    }

    public boolean wordBreak3(String s, Set<String> wordDict, int start) {
        if (start == s.length()) {
            return true;
        }
        for (int end = start + 1; end <= s.length(); end++) {
            if (wordDict.contains(s.substring(start, end)) && wordBreak3(s, wordDict, end)) {
                return true;
            }
        }
        return false;
    }
    
    //记忆化回溯
    public boolean wordBreak4(String s, List<String> wordDict) {
        return wordBreak4(s, new HashSet(wordDict), 0, new Boolean[s.length()]);
    }

    public boolean wordBreak4(String s, Set<String> wordDict, int start, Boolean[] memo) {
        if (start == s.length()) {
            return true;
        }
        if (memo[start] != null) {
            return memo[start];
        }
        for (int end = start + 1; end <= s.length(); end++) {
            if (wordDict.contains(s.substring(start, end)) && wordBreak4(s, wordDict, end, memo)) {
                return memo[start] = true;
            }
        }
        return memo[start] = false;
    }
```



### 动态规划

用 dp[i] 表示字符串 s[0,i) 能否由 wordDict 构成。对应分治思路一

```java
    public boolean wordBreak2(String s, List<String> wordDict) {
        Set<String> wordSet = new HashSet<>(wordDict);
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true;
        for (int i = 1; i <= s.length(); i++) {
            for (int j = 0; j < i; j++) {
                dp[i] = dp[j] && wordSet.contains(s.substring(j, i));
                if (dp[i]) {
                    break;
                }
            }
        }
        return dp[s.length()];
    }

```

用 dp[i] 表示字符串 s[i,len) 能否由 wordDict 构成。对应分治思路二

```java
    public boolean wordBreak2_1(String s, List<String> wordDict) {
        Set<String> wordSet = new HashSet<>(wordDict);
        int len = s.length();
        boolean[] dp = new boolean[len + 1];
        dp[len] = true;
        for (int i = len - 1; i >= 0; i--) {
            for (int j = len; j >= i; j--) {
                dp[i] = dp[j] && wordSet.contains(s.substring(i, j));
                if (dp[i]) {
                    break;
                }
            }
        }
        return dp[0];
    }
```


### BFS

将字符串可视化成一棵树，每一个节点是用 endend 为结尾的前缀字符串。当两个节点之间的所有节点都对应了字典中一个有效字符串时，两个节点可以被连接。

为了形成这样的一棵树，我们从给定字符串的第一个字符开始（比方说 ss ），将它作为树的根部，开始找所有可行的以该字符为首字符的可行子串。进一步的，将每一个子字符串的结束字符的下标（比方说 ii）放在队列的尾部供宽搜后续使用。

每次我们从队列最前面弹出一个元素，并考虑字符串 s(i+1,end)s(i+1,end) 作为原始字符串，并将当前节点作为树的根。这个过程会一直重复，直到队列中没有元素。如果字符串最后的元素可以作为树的一个节点，这意味着初始字符串可以被拆分成多个给定字典中的子字符串。

```java
    public boolean wordBreak5(String s, List<String> wordDict) {
        Set<String> wordDictSet = new HashSet<>(wordDict);
        Queue<Integer> queue = new LinkedList<>();
        int[] visited = new int[s.length()];
        queue.add(0);
        while (!queue.isEmpty()) {
            int start = queue.remove();
            if (visited[start] == 0) {
                for (int end = start + 1; end <= s.length(); end++) {
                    if (wordDictSet.contains(s.substring(start, end))) {
                        queue.add(end);
                        if (end == s.length()) {
                            return true;
                        }
                    }
                }
                visited[start] = 1;
            }
        }
        return false;
    }

```
