# 310. Minimum Height Trees(M)

[310. 最小高度树](https://leetcode-cn.com/problems/minimum-height-trees/)

## 题目描述(中等)

对于一个具有树特征的无向图，我们可选择任何一个节点作为根。图因此可以成为树，在所有可能的树中，具有最小高度的树被称为最小高度树。给出这样的一个图，写出一个函数找到所有的最小高度树并返回他们的根节点。

格式

该图包含 n 个节点，标记为 0 到 n - 1。给定数字 n 和一个无向边 edges 列表（每一个边都是一对标签）。

你可以假设没有重复的边会出现在 `edges` 中。由于所有的边都是无向边， [0, 1]和 [1, 0] 是相同的，因此不会同时出现在 `edges` 里。

示例 1:
```
输入: n = 4, edges = [[1, 0], [1, 2], [1, 3]]

        0
        |
        1
       / \
      2   3 

输出: [1]
```
示例 2:
```
输入: n = 6, edges = [[0, 3], [1, 3], [2, 3], [4, 3], [5, 4]]

     0  1  2
      \ | /
        3
        |
        4
        |
        5 

输出: [3, 4]
```
**说明**:
- 根据树的定义，树是一个无向图，其中任何两个顶点只通过一条路径连接。 换句话说，一个任何没有简单环路的连通图都是一棵树。
- 树的高度是指根节点和叶子节点之间最长向下路径上边的数量。

## 思路

- 遍历每个节点作为根节点求树的高度
- 

## 解决方法

### 遍历节点作为根节点

遍历所有节点作为根节点，求对应树的高度

**超时**

```java
  public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        List<Integer>[] adjacentList = new List[n];
        for (int i = 0; i < n; i++) {
            adjacentList[i] = new ArrayList<>();
        }
        for (int[] edge : edges) {
            int u = edge[0], v = edge[1];
            adjacentList[u].add(v);
            adjacentList[v].add(u);
        }
        int minHeight = Integer.MAX_VALUE;
        Map<Integer, List<Integer>> map = new HashMap<>();
        for (int i = 0; i < n; i++) {
            int height = 0;
            boolean[] visited = new boolean[n];
            Queue<Integer> queue = new LinkedList<>();
            queue.add(i);
            visited[i] = true;
            while (!queue.isEmpty()) {
                height++;
                int size = queue.size();
                for (int j = 0; j < size; j++) {
                    int k = queue.poll();
                    for (int adj : adjacentList[k]) {
                        if (!visited[adj]) {
                            queue.add(adj);
                            visited[adj] = true;
                        }
                    }
                }
            }
            if (height <= minHeight) {
                minHeight = height;
                List<Integer> list = map.getOrDefault(height, new ArrayList<>());
                list.add(i);
                map.put(height, list);
            }

        }
        return map.get(minHeight);
    }
```


### BFS 去除边缘节点

无向简单图，要求找出最高树的节点

采用分层剥削的方法，每次去除一层叶子节点，最后只剩下1或者2节点时候就是最小高度树的根节点

```java
  public List<Integer> findMinHeightTrees1(int n, int[][] edges) {
        List<Integer>[] adjacentList = new List[n];
        Set<Integer> nodes = new HashSet<>();
        int[] degree = new int[n];
        for (int i = 0; i < n; i++) {
            adjacentList[i] = new ArrayList<>();
            nodes.add(i);
        }
        for (int[] edge : edges) {
            int u = edge[0], v = edge[1];
            adjacentList[u].add(v);
            adjacentList[v].add(u);
            degree[u]++;
            degree[v]++;
        }
        while (nodes.size() > 2) {
            List<Integer> removeList = new ArrayList<>();
            for (int i = 0; i < n; i++) {
                if (degree[i] == 1) {
                    removeList.add(i);
                }
            }
            for (Integer leaf : removeList) {
                degree[leaf]--;
                nodes.remove(leaf);
                for (int adj : adjacentList[leaf]) {
                    degree[adj]--;
                }
            }
        }
        return new ArrayList<>(nodes);
    }
```

```java
  public List<Integer> findMinHeightTrees2(int n, int[][] edges) {
        Set<Integer> nodes = new HashSet<>();
        List<Integer>[] adjacentList = new List[n];
        for (int i = 0; i < n; i++) {
            adjacentList[i] = new ArrayList<>();
            nodes.add(i);
        }
        for (int[] edge : edges) {
            int u = edge[0], v = edge[1];
            adjacentList[u].add(v);
            adjacentList[v].add(u);
        }
        List<Integer> leaves = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            if (adjacentList[i].size() == 1) {
                leaves.add(i);
            }
        }
        int size = n;
        while (size > 2) {
            size -= leaves.size();
            List<Integer> newLeaves = new ArrayList<>();
            for (Integer leaf : leaves) {
                nodes.remove(leaf);
                for (Integer adj : adjacentList[leaf]) {
                    adjacentList[adj].remove(leaf);
                    if (adjacentList[adj].size() == 1) {
                        newLeaves.add(adj);
                    }
                }
            }
            leaves = newLeaves;
        }
        return new ArrayList<>(nodes);
    }
```