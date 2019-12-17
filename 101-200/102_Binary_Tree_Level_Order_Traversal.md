# 102. Binary Tree Level Order Traversal(M)

[102. 二叉树的层次遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

## 题目描述(中等)

给定一个二叉树，返回其按层次遍历的节点值。 （即逐层地，从左到右访问所有节点）。
```
例如:
给定二叉树: [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回其层次遍历结果：

[
  [3],
  [9,20],
  [15,7]
]
```

## 思路

有两种通用的遍历树的策略：

- 深度优先搜索（DFS）

在这个策略中，采用深度作为优先级，以便从跟开始一直到达某个确定的叶子，然后再返回根到达另一个分支。

深度优先搜索策略又可以根据根节点、左孩子和右孩子的相对顺序被细分为先序遍历，中序遍历和后序遍历。

- 宽度优先搜索（BFS）

按照高度顺序一层一层的访问整棵树，高层次的节点将会比低层次的节点先被访问到。


## 解决方法

### BFS

将树上顶点按照层次依次放入队列结构中

```java
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> listList = new ArrayList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        if (root == null) {
            return listList;
        }
        queue.add(root);
        while (!queue.isEmpty()) {
            List<Integer> list = new ArrayList<>();
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode p = queue.poll();
                list.add(p.val);
                if (p.left != null) {
                    queue.add(p.left);
                }
                if (p.right != null) {
                    queue.add(p.right);
                }
            }
            listList.add(list);
        }
        return listList;
    }
```

时间复杂度：O(N)，因为每个节点恰好会被运算一次。

空间复杂度：O(N)，保存输出结果的数组包含 N 个节点的值。

### DFS

```java
    public List<List<Integer>> levelOrder1(TreeNode root) {
        List<List<Integer>> listList = new ArrayList<>();
        levelOrder(root, listList, 0);
        return listList;
    }

    public void levelOrder(TreeNode p, List<List<Integer>> lines, int depth) {
        if (p == null) {
            return;
        }
        if (depth + 1 > lines.size()) {
            lines.add(new ArrayList<>());
        }
        List<Integer> line = lines.get(depth);
        line.add(p.val);
        levelOrder(p.left, lines, depth + 1);
        levelOrder(p.right, lines, depth + 1);

    }
```

时间复杂度：O(N)，因为每个节点恰好会被运算一次。

空间复杂度：O(N)，保存输出结果的数组包含 N 个节点的值。