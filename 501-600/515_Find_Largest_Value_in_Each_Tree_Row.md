
# 515. Find Largest Value in Each Tree Row(M)

[515. 在每个树行中找最大值](https://leetcode-cn.com/problems/find-largest-value-in-each-tree-row/)

## 题目描述(中等)

您需要在二叉树的每一行中找到最大的值。

示例：
```
输入: 

          1
         / \
        3   2
       / \   \  
      5   3   9 

输出: [1, 3, 9]
```

## 思路

- BFS
- DFS

## 解决方法

### BFS

层次遍历过程中，比较得到最大值，加入结果集

```java
    public List<Integer> largestValues(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        if (root == null) {
            return list;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            int max = Integer.MIN_VALUE;
            for (int i = 0; i < size; i++) {
                TreeNode p = queue.poll();
                max = Math.max(p.val, max);
                if (p.left != null) {
                    queue.offer(p.left);
                }
                if (p.right != null) {
                    queue.offer(p.right);
                }
            }
            list.add(max);
        }
        return list;
    }
```

### DFS

深度优先遍历过程中，根据当前深度，与当前深度的当前最大值比较更新

```java
    public List<Integer> largestValues1(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        dfs(root, list, 0);
        return list;
    }

    public void dfs(TreeNode p, List<Integer> list, int depth) {
        if (p == null) {
            return;
        }
        if (depth == list.size()) {
            list.add(p.val);
        } else if (list.get(depth) < p.val) {
            list.set(depth, p.val);
        }
        dfs(p.left, list, depth + 1);
        dfs(p.right, list, depth + 1);
    }
```