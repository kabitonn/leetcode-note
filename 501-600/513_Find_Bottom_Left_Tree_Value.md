
# 513. Find Bottom Left Tree Value(M)

[513. 找树左下角的值](https://leetcode-cn.com/problems/find-bottom-left-tree-value/)

## 题目描述(中等)

给定一个二叉树，在树的最后一行找到最左边的值。

示例 1:
```
输入:

    2
   / \
  1   3

输出:
1
``` 

示例 2:
```
输入:

        1
       / \
      2   3
     /   / \
    4   5   6
       /
      7

输出:
7
``` 

**注意**: 您可以假设树（即给定的根节点）不为 NULL。


## 思路

- BFS
- DFS


## 解决方法

### BFS

层次遍历，记录每层第一个节点即可

```java
        public int findBottomLeftValue(TreeNode root) {
        if (root == null) {
            return 0;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int prev = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode p = queue.poll();
                if (i == 0) {
                    prev = p.val;
                }
                if (p.left != null) {
                    queue.offer(p.left);
                }
                if (p.right != null) {
                    queue.offer(p.right);
                }
            }
        }
        return prev;
    }
```

### DFS

List记录每层第一个节点，遍历完后最后一个记录便是最深层的最左叶子结点

不采用List容器记录节点值，也可采用两个类变量来记录最大深度和左下角节点

```java
    public int findBottomLeftValue1(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        dfs(root, list, 0);
        return list.get(list.size() - 1);
    }

    public void dfs(TreeNode p, List<Integer> list, int depth) {
        if (p == null) {
            return;
        }
        if (depth == list.size()) {
            list.add(p.val);
        }
        dfs(p.left, list, depth + 1);
        dfs(p.right, list, depth + 1);
    }
```