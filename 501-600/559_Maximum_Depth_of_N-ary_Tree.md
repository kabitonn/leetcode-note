
# 559. Maximum Depth of N-ary Tree(E)
 
[559. N叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-n-ary-tree/)

## 题目描述(简单)

给定一个 N 叉树，找到其最大深度。

最大深度是指从根节点到最远叶子节点的最长路径上的节点总数。

例如，给定一个 3叉树 :

![](../as/../assets/leetcode-note/501-600/559-p-1.png)

我们应返回其最大深度，3。

**说明**:
- 树的深度不会超过 1000。
- 树的节点总不会超过 5000。



## 思路

- DFS
- BFS

## 解决方法

### DFS

```java
    public int maxDepth(Node root) {
        if (root == null) {
            return 0;
        }
        int max = 0;
        for (Node p : root.children) {
            max = Math.max(max, maxDepth(p));
        }
        return max + 1;
    }
```

### BFS

```java
    public int maxDepth1(Node root) {
        if (root == null) {
            return 0;
        }
        Queue<Node> queue = new LinkedList<>();
        queue.offer(root);
        int depth = 0;
        while (!queue.isEmpty()) {
            depth++;
            int size = queue.size();
            Node p;
            for (int i = 0; i < size; i++) {
                p = queue.poll();
                for (Node child : p.children) {
                    queue.offer(child);
                }
            }
        }
        return depth;
    }
```