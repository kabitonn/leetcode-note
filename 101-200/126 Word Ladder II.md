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
相比上述方法一快了一点

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
相比上述方法一快了一点

### DFS 记忆化 深度

![](/assets/101-200/126-s-4-1.png)

假如我们在考虑上图中黄色节点的相邻节点，发现第三层的 abc 在第二层已经考虑过了。所以第三层的 abc 其实不用再考虑了，第三层的 abc 后边的结构一定和第二层后边的结构一样，因为要找最短的路径，所以如果产生了最短路径，一定是第二层的 abc 首先达到结束单词。

所以在考虑第 k 层的某一个单词，如果这个单词在第 1 到 k-1 层已经出现过，就不继续向下探索了。

```java
    public List<List<String>> findLadders3(String beginWord, String endWord, List<String> wordList) {
        List<List<String>> listList = new ArrayList<>();
        if (!wordList.contains(endWord)) {
            return listList;
        }
        Map<String, Integer> levelMap = new HashMap<>();
        Map<String, List<String>> adjacentMap = new HashMap<>();
        findLevelAdjacent3(beginWord, endWord, wordList, adjacentMap, levelMap);
        List<String> path = new ArrayList<>();
        path.add(beginWord);
        findLadders3(listList, path, beginWord, endWord, wordList, adjacentMap, levelMap);
        return listList;
    }

    public void findLadders3(List<List<String>> listList, List<String> path, String beginWord, String endWord, List<String> wordList, Map<String, List<String>> adjacentMap, Map<String, Integer> levelMap) {
        if (beginWord.equals(endWord)) {
            listList.add(new ArrayList<>(path));
            return;
        }
        List<String> adjacentList = adjacentMap.getOrDefault(beginWord, new ArrayList<>());
        for (String word : adjacentList) {
            if (levelMap.get(beginWord) + 1 == levelMap.get(word)) {
                path.add(word);
                findLadders3(listList, path, word, endWord, wordList, adjacentMap, levelMap);
                path.remove(path.size() - 1);
            }
        }
    }

    public void findLevelAdjacent3(String beginWord, String endWord, List<String> wordList, Map<String, List<String>> adjacentMap, Map<String, Integer> levelMap) {

        Queue<String> queue = new LinkedList<>();
        queue.add(beginWord);
        levelMap.put(beginWord, 0);
        Set<String> wordSet = new HashSet<>(wordList);
        boolean found = false;
        int depth = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            depth++;
            for (int i = 0; i < size; i++) {
                String word = queue.poll();
                List<String> adjacentList = getAdjacentList(word, wordSet);
                adjacentMap.put(word, adjacentList);
                for (String adjacent : adjacentList) {
                    if (!levelMap.containsKey(adjacent)) {
                        levelMap.put(adjacent, depth);
                        if (adjacent.equals(endWord)) {
                            found = true;
                        }
                        queue.add(adjacent);
                    }
                }
            }
            if (found) {
                break;
            }
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

提前存储了单词所在的 depth以及邻接表，方便在 DFS 中来判断我们是否继续深搜。

### DFS 记忆化 剪去非最优邻接单词节点

判断之前是否已经处理过，可以用一个 HashSet 来把之前的节点存起来进行判断。  
java 中遍历 List 过程中，不能对 List 元素进行删除。如果想边遍历边删除，可以借助迭代器。

```java
    public List<List<String>> findLadders4(String beginWord, String endWord, List<String> wordList) {
        List<List<String>> listList = new ArrayList<>();
        if (!wordList.contains(endWord)) {
            return listList;
        }
        Map<String, List<String>> adjacentMap = new HashMap<>();
        findLevelAdjacent4(beginWord, endWord, wordList, adjacentMap);
        List<String> path = new ArrayList<>();
        path.add(beginWord);
        findLadders4(listList, path, beginWord, endWord, wordList, adjacentMap);
        return listList;
    }

    public void findLadders4(List<List<String>> listList, List<String> path, String beginWord, String endWord, List<String> wordList, Map<String, List<String>> adjacentMap) {
        if (beginWord.equals(endWord)) {
            listList.add(new ArrayList<>(path));
            return;
        }
        List<String> adjacentList = adjacentMap.getOrDefault(beginWord, new ArrayList<>());
        for (String word : adjacentList) {
            path.add(word);
            findLadders4(listList, path, word, endWord, wordList, adjacentMap);
            path.remove(path.size() - 1);
        }
    }

    public void findLevelAdjacent4(String beginWord, String endWord, List<String> wordList, Map<String, List<String>> adjacentMap) {

        Queue<String> queue = new LinkedList<>();
        Set<String> visited = new HashSet<>();
        queue.add(beginWord);
        visited.add(beginWord);
        Set<String> wordSet = new HashSet<>(wordList);
        boolean found = false;
        int depth = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            depth++;
            Set<String> subVisited = new HashSet<>();
            for (int i = 0; i < size; i++) {
                String word = queue.poll();
                List<String> adjacentList = getAdjacentList(word, wordSet);
                Iterator<String> iterator = adjacentList.iterator();
                while (iterator.hasNext()) {
                    String adjacent = iterator.next();
                    if (visited.contains(adjacent)) {
                        iterator.remove();
                    } else {
                        subVisited.add(adjacent);
                        queue.add(adjacent);
                        if (adjacent.equals(endWord)) {
                            found = true;
                        }
                    }
                }
                adjacentMap.put(word, adjacentList);
            }
            visited.addAll(subVisited);
            if (found) {
                break;
            }
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

### BFS 存储路径

只用 BFS，一边进行层次遍历，一边就保存结果。当到达结束单词的时候，就把结果存储。省去再进行 DFS 的过程。

BFS 的队列就不去存储 String 了，直接去存到目前为止的路径，也就是一个 List。

```java
    public List<List<String>> findLadders5(String beginWord, String endWord, List<String> wordList) {
        List<List<String>> listList = new ArrayList<>();
        if (!wordList.contains(endWord)) {
            return listList;
        }

        List<String> path = new ArrayList<>();
        Queue<List<String>> queue = new LinkedList<>();
        path.add(beginWord);
        queue.add(path);
        Set<String> wordSet = new HashSet<>(wordList);
        Set<String> visited = new HashSet<>();
        visited.add(beginWord);
        boolean found = false;
        while (!queue.isEmpty()) {
            int size = queue.size();
            Set<String> subVisited = new HashSet<>();
            for (int i = 0; i < size; i++) {
                path = queue.poll();
                String word = path.get(path.size() - 1);
                List<String> adjacentList = getAdjacentList(word, wordSet);
                for (String adjacent : adjacentList) {
                    if (!visited.contains(adjacent)) {
                        subVisited.add(adjacent);
                        path.add(adjacent);
                        if (adjacent.equals(endWord)) {
                            found = true;
                            listList.add(new ArrayList<>(path));
                        }
                        queue.add(new ArrayList<>(path));
                        path.remove(path.size() - 1);
                    }
                }
            }
            visited.addAll(subVisited);
            if (found) {
                break;
            }
        }
        return listList;
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

### 双向BFS


利用 BFS 建立了每个节点的邻居节点

![](/assets/101-200/126-s-7-1.png)

从结束单词反向进行 BFS

![](/assets/101-200/126-s-7-2.png)

当两个方向产生了共同的节点，就是最短路径了

至于每次从哪个方向扩展，可以每次选择需要扩展的节点数少的方向进行扩展

例如上图中，一开始需要向下扩展的个数是 1 个，需要向上扩展的个数是 1 个。个数相等，就向下扩展。然后需要向下扩展的个数就变成了 4 个，而需要向上扩展的个数是 1 个，所以此时向上扩展。接着，需要向上扩展的个数变成了 6 个，需要向下扩展的个数是 4 个，就向下扩展......直到相遇。

**双向扩展的好处**

假设 beginword 和 endword 之间的距离是 d。每个节点可以扩展出 k 个节点。

那么正常的时间复杂就是 $$k^d$$。

双向搜索的时间复杂度就是 $$ k^{d/2} + k^{d/2} $$ 。


```java

```



