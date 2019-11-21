# 127. Word Ladder(M)


[127. 单词接龙](https://leetcode-cn.com/problems/word-ladder/)


## 题目描述(中等)

Given two words (beginWord and endWord), and a dictionary's word list, find the length of shortest transformation sequence from beginWord to endWord, such that:

1. Only one letter can be changed at a time.
2. Each transformed word must exist in the word list. Note that beginWord is not a transformed word.

**Note**:

- Return 0 if there is no such transformation sequence.
- All words have the same length.
- All words contain only lowercase alphabetic characters.
- You may assume no duplicates in the word list.
- You may assume beginWord and endWord are non-empty and are not the same.

Example 1:
```
Input:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

Output: 5

Explanation: As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
return its length 5.
```
Example 2:
```
Input:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

Output: 0

Explanation: The endWord "cog" is not in wordList, therefore no possible transformation.
```

## 思路

- BFS 求邻接点
- 双向BFS



## 解决方法



### BFS 

求出当前节点的所有邻接节点，然后判断

```java
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        if (!wordList.contains(endWord)) {
            return 0;
        }
        Queue<String> queue = new LinkedList<>();
        queue.add(beginWord);
        Set<String> wordSet = new HashSet<>(wordList);
        Set<String> visited = new HashSet<>();
        visited.add(beginWord);
        boolean found = false;
        int depth = 1;
        while (!queue.isEmpty()) {
            int size = queue.size();
            Set<String> subVisited = new HashSet<>();
            for (int i = 0; i < size; i++) {
                String word = queue.poll();
                List<String> adjacentList = getAdjacentList(word, wordSet);
                for (String adjacent : adjacentList) {
                    if (!visited.contains(adjacent)) {
                        subVisited.add(adjacent);
                        if (adjacent.equals(endWord)) {
                            found = true;
                        }
                        queue.add(adjacent);
                    }
                }
            }
            if (subVisited.size() > 0) {
                visited.addAll(subVisited);
                depth++;
            }
            if (found) {
                return depth;
            }
        }
        return 0;
    }

    private List<String> getAdjacentList(String str, Set<String> wordSet) {
        List<String> list = new ArrayList<>();
        char[] s = str.toCharArray();
        for (int i = 0; i < s.length; i++) {
            char old = s[i];
            for (char c = 'a'; c <= 'z'; c++) {
                if (s[i] == c) {
                    continue;
                }
                s[i] = c;
                String t = String.valueOf(s);
                if (wordSet.contains(t)) {
                    list.add(t);
                }
            }
            s[i] = old;
        }
        return list;
    }
```

### 


```
一个单词会产生三个类别，比如 hot 会产生
*ot  
h*t 
ho* 
然后考虑每一个单词，如果产生了相同的类，就把这些单词放在一起

假如我们的单词列表是 ["hot","dot","dog","lot","log","cog"]

考虑 hot,当前的分类结果如下
*ot -> [hot]
h*t -> [hot]
ho* -> [hot]

再考虑 dot,当前的分类结果如下
*ot -> [hot dot]
h*t -> [hot]
ho* -> [hot]

d*t -> [dot]
do* -> [dot]

再考虑 dog,当前的分类结果如下
*ot -> [hot dot]
h*t -> [hot]
ho* -> [hot]

d*t -> [dot]
do* -> [dot dog]

*og -> [dog]
d*g -> [dog]

再考虑 lot,当前的分类结果如下
*ot -> [hot dot lot]
h*t -> [hot]
ho* -> [hot]

d*t -> [dot]
do* -> [dot dog]

*og -> [dog]
d*g -> [dog]

l*t -> [lot]
lo* -> [lot]

然后把每个单词都放到对应的类中，这样最后找当前单词邻居节点的时候就方便了。
比如找 hot 的邻居节点，因为它可以产生 *ot， h*t， ho* 三个类别，所有它的相邻节点就是上边分好类的相应单词
```
根据中间pattern可以得到每个节点在wordList中的邻接节点

```java
    public int ladderLength1(String beginWord, String endWord, List<String> wordList) {
        if (!wordList.contains(endWord)) {
            return 0;
        }
        Map<String, List<String>> adjacentMap = getAdjacentMap(beginWord, wordList);

        Queue<String> queue = new LinkedList<>();
        queue.add(beginWord);
        Set<String> visited = new HashSet<>();
        visited.add(beginWord);
        int depth = 1;
        while (!queue.isEmpty()) {
            int size = queue.size();
            boolean hasVisited = false;
            for (int i = 0; i < size; i++) {
                String word = queue.poll();
                List<String> adjacentList = adjacentMap.get(word);
                for (String adjacent : adjacentList) {
                    if (!visited.contains(adjacent)) {
                        visited.add(adjacent);
                        hasVisited = true;
                        if (adjacent.equals(endWord)) {
                            return depth + 1;
                        }
                        queue.add(adjacent);
                    }
                }
            }
            if (hasVisited) {
                depth++;
            }
        }
        return 0;
    }

    private Map<String, List<String>> getAdjacentMap(String begin, List<String> wordList) {
        List<String> words = new ArrayList<>(wordList);
        words.add(begin);
        Map<String, List<String>> patternMap = new HashMap<>();
        words.forEach(word -> {
            char[] s = word.toCharArray();
            for (int i = 0; i < word.length(); i++) {
                char old = s[i];
                s[i] = '*';
                String p = String.valueOf(s);
                List<String> adjacentList = patternMap.getOrDefault(p, new ArrayList<>());
                adjacentList.add(word);
                patternMap.put(p, adjacentList);
                s[i] = old;
            }
        });
        Map<String, List<String>> adjacentMap = new HashMap<>();
        for (String word : words) {
            char[] s = word.toCharArray();
            List<String> list = new ArrayList<>();
            for (int i = 0; i < s.length; i++) {
                char old = s[i];
                s[i] = '*';
                String p = String.valueOf(s);
                List<String> patternList = patternMap.getOrDefault(p, new ArrayList<>());
                for (String adj : patternList) {
                    if (adj.equals(word)) {
                        continue;
                    }
                    list.add(adj);
                }
                s[i] = old;
            }
            adjacentMap.put(word, list);
        }
        return adjacentMap;
    }
```

### 双向搜索 迭代

```java
    public int ladderLength2(String beginWord, String endWord, List<String> wordList) {

        Set<String> wordSet = new HashSet<>(wordList);
        if (!wordSet.contains(endWord)) {
            return 0;
        }
        Set<String> beginSet = new HashSet<>(), endSet = new HashSet<>();
        int depth = 1;
        HashSet<String> visited = new HashSet<>();
        beginSet.add(beginWord);
        endSet.add(endWord);
        while (!beginSet.isEmpty() && !endSet.isEmpty()) {
            if (beginSet.size() > endSet.size()) {
                Set<String> set = beginSet;
                beginSet = endSet;
                endSet = set;
            }

            Set<String> temp = new HashSet<>();
            for (String word : beginSet) {
                char[] s = word.toCharArray();

                for (int i = 0; i < s.length; i++) {
                    char old = s[i];
                    for (char c = 'a'; c <= 'z'; c++) {
                        s[i] = c;
                        String target = String.valueOf(s);
                        if (endSet.contains(target)) {
                            return depth + 1;
                        }
                        if (!visited.contains(target) && wordSet.contains(target)) {
                            temp.add(target);
                            visited.add(target);
                        }
                    }
                    s[i] = old;
                }
            }

            beginSet = temp;
            depth++;
        }
        return 0;
    }
```


### 双向搜索 BFS 递归

```java
    //因为把 beginWord 和 endWord 都加入了路径，所以初始化 2
    private int len = 2;

    public int ladderLength3(String beginWord, String endWord, List<String> wordList) {
        if (!wordList.contains(endWord)) {
            return 0;
        }
        // 利用 BFS 得到所有的邻居节点
        Set<String> set1 = new HashSet<>();
        set1.add(beginWord);
        Set<String> set2 = new HashSet<>();
        set2.add(endWord);
        Set<String> wordSet = new HashSet<>(wordList);
        //最后没找到返回 0
        if (!twoEndBfs(set1, set2, wordSet)) {
            return 0;
        }
        return len;
    }

    private boolean twoEndBfs(Set<String> set1, Set<String> set2, Set<String> wordSet) {
        if (set1.isEmpty() || set2.isEmpty()) {
            return false;
        }
        // set1 的数量多，就反向扩展
        if (set1.size() > set2.size()) {
            return twoEndBfs(set2, set1, wordSet);
        }
        // 将已经访问过单词删除
        wordSet.removeAll(set1);
        wordSet.removeAll(set2);
        // 保存新扩展得到的节点
        Set<String> set = new HashSet<>();
        for (String word : set1) {
            // 遍历每一位
            for (int i = 0; i < word.length(); i++) {
                char[] s = word.toCharArray();

                // 尝试所有字母
                for (char ch = 'a'; ch <= 'z'; ch++) {
                    if (s[i] == ch) {
                        continue;
                    }
                    s[i] = ch;
                    String adj = new String(s);
                    if (set2.contains(adj)) {
                        return true;
                    }
                    // 如果还没有相遇，并且新的单词在 word 中，那么就加到 set 中
                    if (wordSet.contains(adj)) {
                        set.add(adj);
                    }
                }
            }
        }
        len++;
        // 一般情况下新扩展的元素会多一些，所以我们下次反方向扩展 set2
        return twoEndBfs(set2, set, wordSet);
    }
```
