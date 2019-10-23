# 210. Course Schedule II(M)

[210. 课程表 II](https://leetcode-cn.com/problems/course-schedule-ii/)

## 题目描述(中等)

There are a total of n courses you have to take, labeled from 0 to n-1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

Given the total number of courses and a list of prerequisite **pairs**, return the ordering of courses you should take to finish all courses.

There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.

Example 1:
```
Input: 2, [[1,0]] 
Output: [0,1]
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished   
             course 0. So the correct course order is [0,1] .
```
Example 2:
```
Input: 4, [[1,0],[2,0],[3,1],[3,2]]
Output: [0,1,2,3] or [0,2,1,3]
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both     
             courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0. 
             So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3] .
```
**Note**:

1. The input prerequisites is a graph represented by a list of edges, not adjacency matrices. Read more about how a graph is represented.
2. You may assume that there are no duplicate edges in the input prerequisites.


## 思路

拓扑排序

- BFS 
- DFS

## 解决方法

### BFS

1、邻接表：通过结点的索引，我们能够得到这个结点的后继结点；

2、入度数组：通过结点的索引，我们能够得到指向这个结点的结点个数。

对依赖关系生成入度表和邻接表，对于入度为0的节点为可行节点，加入可行队列；依次取出队首加入拓扑排序的结果数组，对其的相邻节点入度减一，并判断入度是否可入队列，直到队列为空

1、在开始排序前，扫描对应的存储空间（使用邻接表），将入度为 0 的结点放入队列。

2、只要队列非空，就从队首取出入度为 0 的结点，将这个结点输出到结果集中，并且将这个结点的所有邻接结点（它指向的结点）的入度减 1，在减 1 以后，如果这个被减 1 的结点的入度为 0 ，就继续入队。

3、当队列为空的时候，检查结果集中的顶点个数是否和课程数相等即可。



```java
    public int[] findOrder(int numCourses, int[][] prerequisites) {
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
        int[] topo = new int[numCourses];
        int seq = 0;
        while (!queue.isEmpty()) {
            int pre = queue.poll();
            topo[seq++] = pre;
            for (int course : adjacentList[pre]) {
                if (--indegree[course] == 0) {
                    queue.add(course);
                }
            }
        }
        return seq == numCourses ? topo : new int[0];
    }

```


### DFS

第 1 步：构建逆邻接表；

第 2 步：递归处理每一个还没有被访问的结点，具体做法很简单：对于一个结点来说，先输出指向它的所有顶点，再输出自己。

第 3 步：如果这个顶点还没有被遍历过，就递归遍历它，把所有指向它的结点都输出了，再输出自己。注意：当访问一个结点的时候，应当先递归访问它的前驱结点，直至前驱结点没有前驱结点为止。


```java
    public int[] findOrder1(int numCourses, int[][] prerequisites) {
        List<Integer>[] adjacentList = new List[numCourses];
        for (int i = 0; i < numCourses; i++) {
            adjacentList[i] = new ArrayList<>();
        }
        for (int[] pre : prerequisites) {
            adjacentList[pre[1]].add(pre[0]);
        }
        int[] marked = new int[numCourses];
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < numCourses; i++) {
            if (hasCircle(i, adjacentList, marked, stack)) {
                return new int[0];
            }
        }
        int[] seq = new int[numCourses];
        //那个没有前驱的课程，一定在栈顶，所以课程学习的顺序就应该是从栈顶到栈底
        for (int i = 0; i < numCourses; i++) {
            seq[i] = stack.pop();
        }
        return seq;
    }

    /**
     * @param node         当前遍历节点
     * @param adjacentList 邻接表
     * @param marked       -1 其他节点已遍历过， 1 当前节点遍历过程中 0 还未遍历
     * @param stack        存储拓扑排序的逆序，有指向该节点的先入栈
     * @return boolean
     * @date 2019/10/10 21:21
     */
    public boolean hasCircle(int node, List<Integer>[] adjacentList, int[] marked, Stack<Integer> stack) {
        if (marked[node] == 1) {
            return true;
        }
        if (marked[node] == -1) {
            return false;
        }
        marked[node] = 1;
        for (int course : adjacentList[node]) {
            if (hasCircle(course, adjacentList, marked, stack)) {
                return true;
            }
        }
        //node的所有后继结点都访问完了，都没有存在环，则这个结点就可以被标记为已经访问结束
        stack.push(node);
        marked[node] = -1;
        return false;
    }
```