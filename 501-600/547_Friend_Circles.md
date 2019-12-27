
# 547. Friend Circles(M)

[547. 朋友圈](https://leetcode-cn.com/problems/friend-circles/)

## 题目描述(中等)

班上有 N 名学生。其中有些人是朋友，有些则不是。他们的友谊具有是传递性。如果已知 A 是 B 的朋友，B 是 C 的朋友，那么我们可以认为 A 也是 C 的朋友。所谓的朋友圈，是指所有朋友的集合。

给定一个 **N * N** 的矩阵 **M**，表示班级中学生之间的朋友关系。如果M[i][j] = 1，表示已知第 i 个和 j 个学生互为朋友关系，否则为不知道。你必须输出所有学生中的已知的朋友圈总数。

示例 1:
```
输入: 
[[1,1,0],
 [1,1,0],
 [0,0,1]]
输出: 2 
说明：已知学生0和学生1互为朋友，他们在一个朋友圈。
第2个学生自己在一个朋友圈。所以返回2。
```

示例 2:
```
输入: 
[[1,1,0],
 [1,1,1],
 [0,1,1]]
输出: 1
说明：已知学生0和学生1互为朋友，学生1和学生2互为朋友，所以学生0和学生2也是朋友，所以他们三个在一个朋友圈，返回1。
```

**注意**：
- N 在[1,200]的范围内。
- 对于所有学生，有M[i][i] = 1。
- 如果有M[i][j] = 1，则有M[j][i] = 1。



## 思路

连通分支个数



## 解决方法

### 暴力遍历

遍历将属于一个连通分支的映射修改为同一个标记，最后统计标记数目

```java
    public int findCircleNum(int[][] M) {
        int N = M.length;
        int[] map = new int[N];
        for (int i = 0; i < N; i++) {
            map[i] = i;
        }
        for (int i = 0; i < N; i++) {
            for (int j = i + 1; j < N; j++) {
                if (M[i][j] == 0) {
                    continue;
                }
                int c = map[j];
                for (int k = 0; k < N; k++) {
                    if (map[k] == c) {
                        map[k] = map[i];
                    }
                }
            }
        }
        Set<Integer> set = new HashSet<>();
        for (int c : map) {
            set.add(c);
        }
        return set.size();
    }

```

### DFS 矩阵节点

```java
    public int findCircleNum1(int[][] M) {
        int num = 0;
        int N = M.length;
        boolean[][] visited = new boolean[N][N];
        for (int i = 0; i < N; i++) {
            for (int j = i; j < N; j++) {
                if (M[i][j] == 0 || visited[i][j]) {
                    continue;
                }
                num++;
                dfs1(M, i, j, visited);
            }
        }

        return num;
    }

    public void dfs1(int[][] M, int i, int j, boolean[][] visited) {
        if (visited[i][j] || M[i][j] == 0) {
            return;
        }
        visited[i][j] = true;
        visited[j][i] = true;
        for (int k = 0; k < M.length; k++) {
            dfs1(M, j, k, visited);
        }
    }
```

### DFS 图的节点

从每个未被访问的节点开始深搜，每开始一次搜索就增加计数器一次

```java
    public int findCircleNum3(int[][] M) {
        int N = M.length;
        boolean[] mapped = new boolean[N];
        int num = 0;
        for (int i = 0; i < N; i++) {
            if (!mapped[i]) {
                dfs3(M, i, mapped);
                num++;
            }
        }
        return num;
    }

    public void dfs3(int[][] M, int i, boolean[] mapped) {
        if (mapped[i]) {
            return;
        }
        mapped[i] = true;
        for (int j = 0; j < M.length; j++) {
            if (M[i][j] == 1) {
                dfs3(M, j, mapped);
            }
        }
    }
```

利用map记录连通分支的标记

```java
    public int findCircleNum4(int[][] M) {
        int N = M.length;
        int[] mapped = new int[N];
        int num = 0;
        for (int i = 0; i < N; i++) {
            if (mapped[i] == 0) {
                dfs4(M, i, mapped, ++num);
            }
        }
        return num;
    }

    public void dfs4(int[][] M, int i, int[] mapped, int num) {
        if (mapped[i] != 0) {
            return;
        }
        mapped[i] = num;
        for (int j = 0; j < M.length; j++) {
            if (M[i][j] == 1) {
                dfs4(M, j, mapped, num);
            }
        }
    }

```


### BFS

从一个特定点开始，访问所有邻接的节点。然后对于这些邻接节点，我们依然通过访问邻接节点的方式，直到访问所有可以到达的节点

```java
    public int findCircleNum2(int[][] M) {
        Queue<Integer> queue = new LinkedList<>();
        int N = M.length;
        boolean[] visited = new boolean[N];
        int num = 0;
        for (int i = 0; i < N; i++) {
            if (visited[i]) {
                continue;
            }
            num++;
            queue.add(i);
            visited[i] = true;
            while (!queue.isEmpty()) {
                int s = queue.poll();
                for (int j = 0; j < N; j++) {
                    if (!visited[j] && M[s][j] == 1) {
                        queue.add(j);
                        visited[j] = true;
                    }
                }
            }
        }
        return num;
    }
```


