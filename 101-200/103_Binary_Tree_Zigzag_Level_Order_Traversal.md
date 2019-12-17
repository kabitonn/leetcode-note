# 103. Binary Tree Zigzag Level Order Traversal(M)

[103. 二叉树的锯齿形层次遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)


## 题目描述(中等)

给定一个二叉树，返回其节点值的锯齿形层次遍历。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。
```
例如：
给定二叉树 [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回锯齿形层次遍历如下：

[
  [3],
  [20,9],
  [15,7]
]
```

## 思路

- BFS
- DFS


## 解决方法

### BFS

```java
        public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> listList = new ArrayList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        if (root == null) {
            return listList;
        }
        queue.add(root);
        int level = 0;
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
            if ((level & 1) == 1) {
                Collections.reverse(list);
            }
            level++;
            listList.add(list);
        }
        return listList;
    }
```



```java
        public List<List<Integer>> zigzagLevelOrder0(TreeNode root) {
        List<List<Integer>> listList = new ArrayList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        if (root == null) {
            return listList;
        }
        queue.add(root);
        int level = 0;
        while (!queue.isEmpty()) {
            LinkedList<Integer> list = new LinkedList<>();
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode p = queue.poll();
                if (level % 2 == 1) {
                    list.addFirst(p.val);
                } else {
                    list.add(p.val);
                }
                if (p.left != null) {
                    queue.add(p.left);
                }
                if (p.right != null) {
                    queue.add(p.right);
                }
            }
            level++;
            listList.add(list);
        }
        return listList;
    }
```

### DFS

```java
        public List<List<Integer>> zigzagLevelOrder1(TreeNode root) {
        List<List<Integer>> listList = new ArrayList<>();
        zigzagLevelOrder(listList, root, 0);
        return listList;
    }

    private void zigzagLevelOrder(List<List<Integer>> listList, TreeNode p, int depth) {
        if (p == null) {
            return;
        }
        if (depth + 1 > listList.size()) {
            listList.add(new ArrayList<>());
        }
        List<Integer> list = listList.get(depth);
        if ((depth & 1) == 0) {
            list.add(0, p.val);
        } else {
            list.add(p.val);
        }
        zigzagLevelOrder(listList, p.left, depth + 1);
        zigzagLevelOrder(listList, p.right, depth + 1);
    }

```

### 交替栈

用两个栈来模拟从右向左和从左向右的过程。

1. 遍历第二层的节点的顺序是从左到右，而输出第二层节点的顺序是从右到左，这正好满足栈思想；
2. 遍历栈输出第二层节点，同时还需要将第三层的节点按照从右到左的顺序存储。但是不能将第三层的节点存储到这个栈中，因为会打乱第二层节点的顺序，此时我们可以想到创建另一个栈来存储第三层的元素。这样层与层之间就不会产生干扰。
3. 交替使用这两个栈遍历所有的层。


```java
        public List<List<Integer>> zigzagLevelOrder2(TreeNode root) {
        List<List<Integer>> listList = new ArrayList<>();
        if (root == null) {
            return listList;
        }
        Stack<TreeNode> s1 = new Stack<>();
        Stack<TreeNode> s2 = new Stack<>();
        TreeNode p = root;
        s1.push(p);
        while (!s1.isEmpty() || !s2.isEmpty()) {
            List<Integer> list = new ArrayList<>();
            while (!s1.isEmpty()) {
                p = s1.pop();
                list.add(p.val);
                if (p.left != null) {
                    s2.push(p.left);
                }
                if (p.right != null) {
                    s2.push(p.right);
                }
            }
            listList.add(list);
            list = new ArrayList<>();
            while (!s2.isEmpty()) {
                p = s2.pop();
                list.add(p.val);
                if (p.right != null) {
                    s1.push(p.right);
                }
                if (p.left != null) {
                    s1.push(p.left);
                }
            }
            if (!list.isEmpty()) {
                listList.add(list);
            }
        }
        return listList;
    }
```