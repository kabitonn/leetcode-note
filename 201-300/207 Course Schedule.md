# 207. Course Schedule(M)

[207. 课程表](https://leetcode-cn.com/problems/course-schedule/)

## 题目描述(中等)

There are a total of n courses you have to take, labeled from 0 to n-1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

Given the total number of courses and a list of prerequisite **pairs**, is it possible for you to finish all courses?

Example 1:
```
Input: 2, [[1,0]] 
Output: true
Explanation: There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0. So it is possible.
```

Example 2:
```
Input: 2, [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0, and to take course 0 you should
             also have finished course 1. So it is impossible.
```

**Note**:

- The input prerequisites is a graph represented by a list of edges, not adjacency matrices. Read more about how a graph is represented.
- You may assume that there are no duplicate edges in the input prerequisites.


## 思路

判断依赖关系有没有环

- BFS
- DFS

## 解决方法

### BFS 入度 拓扑排序

1、邻接表：通过结点的索引，我们能够得到这个结点的后继结点；

2、入度数组：通过结点的索引，我们能够得到指向这个结点的结点个数。

对依赖关系生成入度表和邻接表，对于入度为0的节点为可行节点，加入可行队列；依次取出队首加入拓扑排序的结果数组，对其的相邻节点入度减一，并判断入度是否可入队列，直到队列为空

1、在开始排序前，扫描对应的存储空间（使用邻接表），将入度为 0 的结点放入队列。

2、只要队列非空，就从队首取出入度为 0 的结点，将这个结点输出到结果集中，并且将这个结点的所有邻接结点（它指向的结点）的入度减 1，在减 1 以后，如果这个被减 1 的结点的入度为 0 ，就继续入队。

3、当队列为空的时候，检查结果集中的顶点个数是否和课程数相等即可。




```java
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        int[] indegree = new int[numCourses];
        List<Integer>[] adjacentList = new List[numCourses];
        for (int i = 0; i < numCourses; i++) {
            adjacentList[i] = new ArrayList<>();
        }
        for (int[] pre : prerequisites) {
            indegree[pre[0]]++;
            adjacentList[pre[1]].add(pre[0]);
        }
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (indegree[i] == 0) {
                queue.add(i);
            }
        }
        int leftCourse = numCourses;
        while (!queue.isEmpty()) {
            int pre = queue.poll();
            leftCourse--;
            for (int course : adjacentList[pre]) {
                if (--indegree[course] == 0) {
                    queue.add(course);
                }
            }
        }
        return leftCourse == 0;
    }

```
时间复杂度 O(N + M)：遍历一个图需要访问所有节点和所有临边，N 和 M 分别为节点数量和临边数量；
空间复杂度 O(N)，为建立邻接矩阵所需额外空间。

### 递归DFS

marked 标记节点状态
- 1 当次DFS中
- -1 已被其他节点的DFS中访问
- 0 未访问

对 numCourses 个节点依次执行 DFS，判断每个节点起步 DFS 是否存在环，若存在环直接返回 true
- 终止条件
    - marked[i] == -1,说明当前访问节点已被其他节点启动的 DFS 访问，无需再重复搜索，直接返回false
    - marked[i] == 1, 说明在本轮 DFS 搜索中节点 i 被第 2 次访问，即 课程安排图有环，直接返回true
- 将当前访问节点 i 对应 marked[i] 置 1，即标记其被本轮 DFS 访问过；
- 递归访问当前节点 i 的所有邻接节点 j，当发现环直接返回 true；
- 当前节点所有邻接节点已被遍历，并没有发现环，则将当前节点 flag 置为 −1 并返回 false。


第 1 步：构建逆邻接表；

第 2 步：递归处理每一个还没有被访问的结点，具体做法很简单：对于一个结点来说，先输出指向它的所有顶点，再输出自己。

第 3 步：如果这个顶点还没有被遍历过，就递归遍历它，把所有指向它的结点都输出了，再输出自己。注意：当访问一个结点的时候，应当先递归访问它的前驱结点，直至前驱结点没有前驱结点为止。



```java
    public boolean canFinish1(int numCourses, int[][] prerequisites) {
        List<Integer>[] adjacentList = new List[numCourses];
        for (int i = 0; i < numCourses; i++) {
            adjacentList[i] = new ArrayList<>();
        }
        for (int[] pre : prerequisites) {
            adjacentList[pre[1]].add(pre[0]);
        }
        int[] marked = new int[numCourses];
        for (int i = 0; i < numCourses; i++) {
            if (hasCircle(i, adjacentList, marked)) {
                return false;
            }
        }
        return true;
    }

    public boolean hasCircle(int node, List<Integer>[] adjacentList, int[] marked) {
        if (marked[node] == 1) {
            return true;
        }
        if (marked[node] == -1) {
            return false;
        }
        marked[node] = 1;
        for (int adj : adjacentList[node]) {
            if (hasCircle(adj, adjacentList, marked)) {
                return true;
            }
        }
        marked[node] = -1;
        return false;
    }
```

时间复杂度 O(N + M)：遍历一个图需要访问所有节点和所有临边，N 和 M 分别为节点数量和临边数量；
空间复杂度 O(N)，为建立邻接矩阵所需额外空间。

