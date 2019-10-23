# 199. Binary Tree Right Side View(M)


[199. 二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view/)


## 题目描述(中等)



## 思路



## 解决方法



### BFS


```java
    public List<Integer> rightSideView(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        List<Integer> list = new ArrayList<>();
        if (root == null) {
            return list;
        }
        queue.add(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode p = queue.poll();
                if (i == size - 1) {
                    list.add(p.val);
                }
                if (p.left != null) {
                    queue.add(p.left);
                }
                if (p.right != null) {
                    queue.add(p.right);
                }
            }
        }
        return list;
    }
```

### DFS 

```java
    public List<Integer> rightSideView1(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        if (root == null) {
            return list;
        }
        levelTraversel(root, list, 1);
        return list;
    }

    public void levelTraversel(TreeNode p, List<Integer> list, int level) {
        if (p == null) {
            return;
        }
        if (level > list.size()) {
            list.add(p.val);
        }
        levelTraversel(p.right, list, level + 1);
        levelTraversel(p.left, list, level + 1);
    }

```

### 迭代DFS

```java
    public List<Integer> rightSideView2(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        if (root == null) {
            return list;
        }
        Stack<TreeNode> nodeStack = new Stack<>();
        Stack<Integer> levelStack = new Stack<>();
        nodeStack.push(root);
        levelStack.push(1);
        while (!nodeStack.isEmpty()) {
            TreeNode p = nodeStack.pop();
            int level = levelStack.pop();
            if (p == null) {
                continue;
            }
            if (level > list.size()) {
                list.add(p.val);
            }
            nodeStack.push(p.left);
            nodeStack.push(p.right);
            levelStack.push(level + 1);
            levelStack.push(level + 1);
        }
        return list;
    }
```

