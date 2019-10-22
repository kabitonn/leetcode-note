# 126. Word Ladder II\(H\)



## 题目描述\(困难\)

Given two words \(beginWord and endWord\), and a dictionary's word list, find all shortest transformation sequence\(s\) from beginWord to endWord, such that:

1. Only one letter can be changed at a time
2. Each transformed word must exist in the word list. Note that beginWord is not a transformed word.

**Note**:

* Return an empty list if there is no such transformation sequence.
* All words have the same length.
* All words contain only lowercase alphabetic characters.
* You may assume no duplicates in the word list.
* You may assume beginWord and endWord are non-empty and are not the same.

Example 1:

```
Input:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

Output:
[
  ["hit","hot","dot","dog","cog"],
  ["hit","hot","lot","log","cog"]
]
```

Example 2:

```
Input:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

Output: []

Explanation: The endWord "cog" is not in wordList, therefore no possible transformation.
```

## 思路

* DFS 
* BFS

## 单词相邻方法

### 遍历不同字符个数判断是否相邻

```java
    private boolean isAdjacent(String s, String t) {
        int count = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) != t.charAt(i)) {
                count++;
            }
            if (count == 2) {
                return false;
            }
        }
        return count == 1;
    }
```

### 获取当前单词所有邻接节点的列表

将要找的节点单词的每个位置换一个字符，然后看更改后的单词在不在 wordList 中

```java
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

### 判断单词的pattern

```java
        Map<String, List<String>> adjacentMap = new HashMap<>();
        wordList.forEach(word -> {
            char[] s = word.toCharArray();
            for (int i = 0; i < word.length(); i++) {
                char old = s[i];
                s[i] = '*';
                String p = String.valueOf(s);
                List<String> adjacentList = adjacentMap.getOrDefault(p, new ArrayList<>());
                adjacentList.add(word);
                adjacentMap.put(p, adjacentList);
                s[i] = old;
            }
        });
```

## 解决方法

### DFS 判断单词是否相邻

```java
    int minPath = Integer.MAX_VALUE;

    public List<List<String>> findLadders(String beginWord, String endWord, List<String> wordList) {
        List<List<String>> listList = new ArrayList<>();
        if (!wordList.contains(endWord)) {
            return listList;
        }
        boolean[] visited = new boolean[wordList.size()];
        List<String> path = new ArrayList<>();
        path.add(beginWord);
        findLadders(listList, path, beginWord, endWord, wordList, visited);
        return listList;
    }

    public void findLadders(List<List<String>> listList, List<String> path, String beginWord, String endWord, List<String> wordList, boolean[] visited) {
        if (beginWord.equals(endWord)) {
            if (minPath > path.size()) {
                listList.clear();
                listList.add(new ArrayList<>(path));
                minPath = path.size();
            } else if (minPath == path.size()) {
                listList.add(new ArrayList<>(path));
            }
            return;
        }
        //当前的长度到达了 minPath,但没有到达结束单词就提前结束
        if (minPath <= path.size()) {
            return;
        }
        for (int i = 0; i < wordList.size(); i++) {
            if (visited[i]) {
                continue;
            }
            String word = wordList.get(i);
            if (isAdjacent(beginWord, word)) {
                visited[i] = true;
                path.add(word);
                findLadders(listList, path, word, endWord, wordList, visited);
                path.remove(path.size() - 1);
                visited[i] = false;
            }
        }
    }

    private boolean isAdjacent(String s, String t) {
        int count = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) != t.charAt(i)) {
                count++;
            }
            if (count == 2) {
                return false;
            }
        }
        return count == 1;
    }
```

![](/assets/101-200/126-s-1-1.png)

### DFS 获取当前单词的相邻单词列表

```java
    int minPath = Integer.MAX_VALUE;

    public List<List<String>> findLadders1(String beginWord, String endWord, List<String> wordList) {
        List<List<String>> listList = new ArrayList<>();
        if (!wordList.contains(endWord)) {
            return listList;
        }
        Set<String> visited = new HashSet<>();
        List<String> path = new ArrayList<>();
        path.add(beginWord);
        visited.add(beginWord);
        findLadders1(listList, path, beginWord, endWord, wordList, visited);
        return listList;
    }

    public void findLadders1(List<List<String>> listList, List<String> path, String beginWord, String endWord, List<String> wordList, Set<String> visited) {
        if (beginWord.equals(endWord)) {
            if (minPath > path.size()) {
                listList.clear();
                listList.add(new ArrayList<>(path));
                minPath = path.size();
            } else if (minPath == path.size()) {
                listList.add(new ArrayList<>(path));
            }
            return;
        }
        //当前的长度到达了 minPath,但没有到达结束单词就提前结束
        if (minPath <= path.size()) {
            return;
        }
        List<String> adjacentList = getAdjacentList(beginWord, new HashSet<>(wordList));
        for (String word : adjacentList) {
            if (visited.contains(word)) {
                continue;
            }
            visited.add(word);
            path.add(word);
            findLadders1(listList, path, word, endWord, wordList, visited);
            path.remove(path.size() - 1);
            visited.remove(word);
        }
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

![](/assets/101-200/126-s-2-1.png)

### DFS pattern判断相邻

```java
    int minPath = Integer.MAX_VALUE;

    public List<List<String>> findLadders2(String beginWord, String endWord, List<String> wordList) {
        List<List<String>> listList = new ArrayList<>();
        if (!wordList.contains(endWord)) {
            return listList;
        }
        Set<String> visited = new HashSet<>();
        List<String> path = new ArrayList<>();
        Map<String, List<String>> adjacentMap = new HashMap<>();
        wordList.forEach(word -> {
            char[] s = word.toCharArray();
            for (int i = 0; i < word.length(); i++) {
                char old = s[i];
                s[i] = '*';
                String p = String.valueOf(s);
                List<String> adjacentList = adjacentMap.getOrDefault(p, new ArrayList<>());
                adjacentList.add(word);
                adjacentMap.put(p, adjacentList);
                s[i] = old;
            }
        });
        path.add(beginWord);
        visited.add(beginWord);
        findLadders2(listList, path, beginWord, endWord, wordList, adjacentMap, visited);
        return listList;
    }

    public void findLadders2(List<List<String>> listList, List<String> path, String beginWord, String endWord, List<String> wordList, Map<String, List<String>> map, Set<String> visited) {
        if (beginWord.equals(endWord)) {
            if (minPath > path.size()) {
                listList.clear();
                listList.add(new ArrayList<>(path));
                minPath = path.size();
            } else if (minPath == path.size()) {
                listList.add(new ArrayList<>(path));
            }
            return;
        }
        //当前的长度到达了 minPath,但没有到达结束单词就提前结束
        if (minPath <= path.size()) {
            return;
        }
        int len = beginWord.length();
        char[] s = beginWord.toCharArray();
        for (int i = 0; i < len; i++) {
            char old = s[i];
            s[i] = '*';
            String p = String.valueOf(s);
            for (String word : map.getOrDefault(p, new ArrayList<>())) {
                if (visited.contains(word)) {
                    continue;
                }
                visited.add(word);
                path.add(word);
                findLadders2(listList, path, word, endWord, wordList, map, visited);
                path.remove(path.size() - 1);
                visited.remove(word);
            }
            s[i] = old;
        }
    }
```

![](/assets/101-200/126-s-3-1.png)



