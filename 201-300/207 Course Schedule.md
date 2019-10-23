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

### BFS 入度

对依赖关系生成入度表和邻接表，对于入度为0的节点为可行节点，加入可行队列；依次取出队首，对其的相邻节点入度减一，并判断入度是否可入队列，直到队列为空

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

### 递归DFS


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
