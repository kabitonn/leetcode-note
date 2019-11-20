
# 433. Minimum Genetic Mutation(M)

[433. 最小基因变化](https://leetcode-cn.com/problems/minimum-genetic-mutation/)

## 题目描述(中等)

一条基因序列由一个带有**8**个字符的字符串表示，其中每个字符都属于 `"A", "C", "G", "T"`中的任意一个。

假设我们要调查一个基因序列的变化。一次基因变化意味着这个基因序列中的一个字符发生了变化。

例如，基因序列由`"AACCGGTT"` 变化至 `"AACCGGTA"` 即发生了一次基因变化。

与此同时，每一次基因变化的结果，都需要是一个合法的基因串，即该结果属于一个基因库。

现在给定3个参数 — `start`, `end`, `bank`，分别代表起始基因序列，目标基因序列及基因库，请找出能够使起始基因序列变化为目标基因序列所需的最少变化次数。如果无法实现目标变化，请返回 -1。

**注意**:
- 起始基因序列默认是合法的，但是它并不一定会出现在基因库中。
- 所有的目标基因序列必须是合法的。
- 假定起始基因序列与目标基因序列是不一样的。

示例 1:
```
start: "AACCGGTT"
end:   "AACCGGTA"
bank: ["AACCGGTA"]

返回值: 1
```
示例 2:
```
start: "AACCGGTT"
end:   "AAACGGTA"
bank: ["AACCGGTA", "AACCGCTA", "AAACGGTA"]

返回值: 2
```

示例 3:
```
start: "AAAAACCC"
end:   "AACCCCCC"
bank: ["AAAACCCC", "AAACCCCC", "AACCCCCC"]

返回值: 3
```

## 思路

连通性
- BFS

## 解决方法

### BFS

BFS遍历得到所有可能的结果，由于找最短路径，若当前遍历过程中找到的节点之前已找到，可忽略，因为必然不是最短路径，

```java
    char[] genes = {'A', 'C', 'G', 'T'};

    public int minMutation(String start, String end, String[] bank) {
        int depth = 0;
        Queue<String> queue = new LinkedList<>();
        Set<String> visited = new HashSet<>();
        Set<String> bankSet = new HashSet<>(Arrays.asList(bank));
        queue.add(start);
        while (!queue.isEmpty()) {
            depth++;
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                String str = queue.poll();
                List<String> adjList = getAdjacentList(str, bankSet);
                for (String adj : adjList) {
                    if (!visited.contains(adj)) {
                        queue.add(adj);
                        if (end.equals(adj)) {
                            return depth;
                        }
                        visited.add(adj);
                    }
                }
            }
        }
        return -1;
    }

    private List<String> getAdjacentList(String str, Set<String> bankSet) {
        char[] s = str.toCharArray();
        List<String> list = new ArrayList<>();
        for (int i = 0; i < s.length; i++) {
            char old = s[i];
            for (int k = 0; k < genes.length; k++) {
                if (s[i] == genes[k]) {
                    continue;
                }
                s[i] = genes[k];
                String adj = String.valueOf(s);
                if (bankSet.contains(adj)) {
                    list.add(adj);
                }
            }
            s[i] = old;
        }
        return list;
    }

```

### 双端BFS

由于单向bfs类似金字塔，越到底层，塔基越大（而众多塔基中只有一点end满足条件），而且其回溯路径也少。所以采用双向bfs,即既从begin->end遍历，又从end->begin遍历，当然每次我们都选用较短队列进行遍历，这样可减少用时。


```java
    char[] genes = {'A', 'C', 'G', 'T'};

    public int minMutation1(String start, String end, String[] bank) {
        Set<String> beginSet = new HashSet<>();
        Set<String> endSet = new HashSet<>();
        Set<String> visited = new HashSet<>();
        Set<String> bankSet = new HashSet<>(Arrays.asList(bank));
        if (!bankSet.contains(end)) {
            return -1;
        }
        beginSet.add(start);
        endSet.add(end);
        int depth = 0;
        while (!beginSet.isEmpty() && !endSet.isEmpty()) {
            if (beginSet.size() > endSet.size()) {
                Set<String> set = beginSet;
                beginSet = endSet;
                endSet = set;
            }
            depth++;
            Set<String> tmp = new HashSet<>();
            for (String str : beginSet) {
                char[] s = str.toCharArray();
                for (int i = 0; i < s.length; i++) {
                    char old = s[i];
                    for (int k = 0; k < genes.length; k++) {
                        if (s[i] == genes[k]) {
                            continue;
                        }
                        s[i] = genes[k];
                        String adj = String.valueOf(s);
                        if (!bankSet.contains(adj)) {
                            continue;
                        }
                        if (endSet.contains(adj)) {
                            return depth;
                        }
                        if (!visited.contains(adj)) {
                            visited.add(adj);
                            tmp.add(adj);
                        }
                    }
                    s[i] = old;
                }
            }
            beginSet = tmp;
        }
        return -1;
    }
```

### DFS 回溯

```java
    public int minMutation2(String start, String end, String[] bank) {
        Set<String> bankSet = new HashSet<>(Arrays.asList(bank));
        if (!bankSet.contains(end)) {
            return -1;
        }
        int min = backtrack(start, end, bankSet, new HashSet<>());
        return min != Integer.MAX_VALUE ? min : -1;
    }

    public int backtrack(String start, String end, Set<String> bank, Set<String> visited) {
        if (start.equals(end)) {
            return 0;
        }
        int min = Integer.MAX_VALUE;
        visited.add(start);
        for (String s : bank) {
            if (isAdjacent(start, s) && !visited.contains(s)) {
                int depth = backtrack(s, end, bank, visited);
                if (depth != Integer.MAX_VALUE) {
                    min = Math.min(min, depth + 1);
                }
            }
        }
        visited.remove(start);
        return min;
    }

    public boolean isAdjacent(String s, String t) {
        int diff = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) != t.charAt(i)) {
                if (++diff > 1) {
                    break;
                }
            }
        }
        return diff == 1;
    }
```