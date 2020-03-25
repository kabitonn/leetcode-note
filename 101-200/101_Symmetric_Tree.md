# 101. Symmetric Tree(E)
[101. 对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)

## 题目描述(简单)

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree [1,2,2,3,4,4,3] is symmetric:
```
    1
   / \
  2   2
 / \ / \
3  4 4  3
``` 

But the following [1,2,2,null,3,null,3] is not:
```
    1
   / \
  2   2
   \   \
   3    3
```
**Note**:
- Bonus points if you could solve it both recursively and iteratively.



## 思路
1. 递归
2. DFS
3. BFS
4. 层次遍历回文序列

## 解决方法

### 递归

判断当前节点是否相等以及左子树和右子树镜像判断

```java
    public boolean isSymmetric(TreeNode root) {
        return isMirror(root, root);
    }

    public boolean isMirror(TreeNode p, TreeNode q) {
        if (p == null && q == null) {
            return true;
        } else if (p == null || q == null) {
            return false;
        }
        if (p.val != q.val) {
            return false;
        }
        return isMirror(p.left, q.right) && isMirror(p.right, q.left);
    }
```
时间复杂度：O(n)，因为我们遍历整个输入树一次，所以总的运行时间为 O(n)，其中 n 是树中结点的总数。
空间复杂度：递归调用的次数受树的高度限制。在最糟糕情况下，树是线性的，其高度为 O(n)。因此，在最糟糕的情况下，由栈上的递归调用造成的空间复杂度为 O(n)。

### DFS 栈
解法一其实就是类似于 DFS 的先序遍历。不同之处是对于 left 子树是正常的先序遍历 根节点 -> 左子树 -> 右子树 的顺序，对于 right 子树的话是 根节点 -> 右子树 -> 左子树 的顺序。

```
public boolean isSymmetric0(TreeNode root) {
        if (root == null) {
            return true;
        }
        Stack<TreeNode> stackLeft = new Stack<>();
        Stack<TreeNode> stackRight = new Stack<>();
        TreeNode curLeft = root.left;
        TreeNode curRight = root.right;
        while (curLeft != null || !stackLeft.isEmpty() || curRight != null || !stackRight.isEmpty()) {
            // 节点不为空一直压栈
            while (curLeft != null) {
                stackLeft.push(curLeft);
                curLeft = curLeft.left;
            }
            while (curRight != null) {
                stackRight.push(curRight);
                curRight = curRight.right;
            }
            //长度不同就返回 false
            if (stackLeft.size() != stackRight.size()) {
                return false;
            }
            // 节点为空，就出栈
            curLeft = stackLeft.pop();
            curRight = stackRight.pop();

            // 当前值判断
            if (curLeft.val != curRight.val) {
                return false;
            }
            curLeft = curLeft.right;
            curRight = curRight.left;
        }
        return true;
    }
```

### BFS 队列

一层一层的遍历两个树，然后判断对应的节点是否相等即可。


```java
    public boolean isSymmetric2(TreeNode root) {
        if (root == null) {
            return true;
        }
        Deque<TreeNode> queue = new LinkedList<>();
        queue.push(root.left);
        queue.push(root.right);
        while (!queue.isEmpty()) {
            TreeNode p = queue.poll();
            TreeNode q = queue.poll();
            if (p == null && q == null) {
                continue;
            } else if (p == null || q == null) {
                return false;
            }
            if (p.val != q.val) {
                return false;
            }
            queue.push(p.left);
            queue.push(q.right);
            queue.push(p.right);
            queue.push(q.left);
        }
        return true;
    }
```

### 层次遍历回文
每层的遍历结果是回文序列

```java
    public boolean isSymmetric1(TreeNode root) {
        Deque<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            List<Integer> line = new ArrayList<>();
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode pNode = queue.poll();
                if (pNode != null) {
                    line.add(pNode.val);
                    queue.add(pNode.left);
                    queue.add(pNode.right);
                } else {
                    line.add(null);
                }
            }
            for (int i = 0, j = line.size() - 1; i < j; i++, j--) {
                if (line.get(i) == null && line.get(j) == null) {
                    continue;
                } else if (line.get(i) == null || line.get(j) == null) {
                    return false;
                } else if (!line.get(i).equals(line.get(j))) {
                    return false;
                }
            }
        }
        return true;
    }
```



