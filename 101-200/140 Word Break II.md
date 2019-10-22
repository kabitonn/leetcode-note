# 140. Word Break II(H)


[140. 单词拆分 II](https://leetcode-cn.com/problems/word-break-ii/)


## 题目描述(困难)

Given a **non-empty** string s and a dictionary wordDict containing a list of **non-empty** words, add spaces in s to construct a sentence where each word is a valid dictionary word. Return all such possible sentences.

**Note**:

- The same word in the dictionary may be reused multiple times in the segmentation.
- You may assume the dictionary does not contain duplicate words.

Example 1:
```
Input:
s = "catsanddog"
wordDict = ["cat", "cats", "and", "sand", "dog"]
Output:
[
  "cats and dog",
  "cat sand dog"
]
```
Example 2:
```
Input:
s = "pineapplepenapple"
wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
Output:
[
  "pine apple pen apple",
  "pineapple pen apple",
  "pine applepen apple"
]
Explanation: Note that you are allowed to reuse a dictionary word.
```
Example 3:
```
Input:
s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
Output:
[]
```

## 思路

- 回溯
- 

## 解决方法



### 回溯

```java
    public List<String> wordBreak(String s, List<String> wordDict) {
        return wordBreak(s, 0, new HashSet<>(wordDict));
    }

    public List<String> wordBreak(String s, int start, Set<String> wordSet) {
        List<String> list = new ArrayList<>();
        if (start == s.length()) {
            list.add("");
        }
        for (int end = start + 1; end <= s.length(); end++) {
            String prefix = s.substring(start, end);
            if (wordSet.contains(prefix)) {
                List<String> strs = wordBreak(s, end, wordSet);
                for (String str : strs) {
                    list.add(prefix + (str.equals("") ? "" : " ") + str);
                }
            }
        }
        return list;
    }
```

时间复杂度：$$O(n^n)$$，考虑最坏情况 ss = "aaaaaaa"，s 的每一个前缀都在字典中，回溯树的大小会达到 $$ n^n $$
空间复杂度：O(n^3)，最坏情况下，回溯的深度可以达到 n 层，每层可能包含 n 个字符串，且每个字符串的长度都为 n 。

### 记忆化回溯

key 是当前考虑字符串的开始下标， value 包含了所有从头开始的所有可行句子。

```java
    public List<String> wordBreak1(String s, List<String> wordDict) {
        return wordBreak1(s, 0, new HashSet<>(wordDict), new HashMap<Integer, List<String>>());
    }

    public List<String> wordBreak1(String s, int start, Set<String> wordSet, Map<Integer, List<String>> map) {
        if (map.containsKey(start)) {
            return map.get(start);
        }
        List<String> list = new ArrayList<>();
        if (start == s.length()) {
            list.add("");
        }
        for (int end = start + 1; end <= s.length(); end++) {
            String prefix = s.substring(start, end);
            if (wordSet.contains(prefix)) {
                List<String> strs = wordBreak1(s, end, wordSet, map);
                for (String str : strs) {
                    list.add(prefix + (str.equals("") ? "" : " ") + str);
                }
            }
        }
        map.put(start, list);
        return list;
    }
```


另一种回溯记忆化思路

key 是当前考虑字符串的结束下标+1， valuevalue 包含了所有从头开始的所有可行句子。


```java
    public List<String> wordBreak2(String s, List<String> wordDict) {
        return wordBreak2(s, s.length(), new HashSet<>(wordDict), new HashMap<Integer, List<String>>());
    }

    public List<String> wordBreak2(String s, int end, Set<String> wordSet, Map<Integer, List<String>> map) {
        if (map.containsKey(end)) {
            return map.get(end);
        }
        List<String> list = new ArrayList<>();
        if (end == 0) {
            return list;
        }
        for (int start = 0; start <= end; start++) {
            String posterfix = s.substring(start, end);
            if (wordSet.contains(posterfix)) {
                if (start == 0) {
                    list.add(posterfix);
                } else {
                    List<String> strs = wordBreak2(s, start, wordSet, map);
                    for (String str : strs) {
                        list.add(str + " " + posterfix);
                    }
                }
            }
        }
        map.put(end, list);
        return list;
    }
```
