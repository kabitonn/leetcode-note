# 094. Binary Tree Inorder Traversal\(M\)

[094. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

## 题目描述\(中等\)

Given a binary tree, return the inorder traversal of its nodes' values.

Example:

```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,3,2]
Follow up: Recursive solution is trivial, could you do it iteratively?
```

## 思路

* 递归
* 迭代
* Morris Traversal

## 解决方法

### 递归

```java
 public void inorder(List<Integer> list, TreeNode p) {
        if (p == null) {
            return;
        }
        inorder(list, p.left);
        list.add(p.val);
        inorder(list, p.right);
    }
```

时间复杂度：O\(n\)。

空间复杂度：O\(h\)，栈消耗，h 是二叉树的高度。

### 迭代

模拟递归

```
        1
      /   \
     2     3
    / \   /
   4   5 6

 push   push   push   pop     pop    push     pop     pop 
|   |  |   |  |_4_|  |   |   |   |  |   |    |   |   |   |  
|   |  |_2_|  |_2_|  |_2_|   |   |  |_5_|    |   |   |   |
|_1_|  |_1_|  |_1_|  |_1_|   |_1_|  |_1_|    |_1_|   |   |
ans                  add 4   add 2           add 5   add 1
[]                   [4]     [4 2]           [4 2 5] [4 2 5 1]
 push   push   pop          pop 
|   |  |   |  |   |        |   |  
|   |  |_6_|  |   |        |   |  
|_3_|  |_3_|  |_3_|        |   |
              add 6        add 3
              [4 2 5 1 6]  [4 2 5 1 6 3]
```

```java
    public List<Integer> inorderTraversal1(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        if (root == null) {
            return list;
        }
        Stack<TreeNode> stack = new Stack<>();
        TreeNode p = root;
        while (p!=null||!stack.isEmpty()) {
            while (p != null) {
                stack.push(p);
                p = p.left;
            }
            p = stack.pop();
            list.add(p.val);
            p = p.right;
        }
        return list;
    }
```

时间复杂度：O\(n\)。

空间复杂度：O\(h\)，栈消耗，h 是二叉树的高度。

### Morris Traversal

左子树最后遍历的节点一定是一个叶子节点，它的左右孩子都是 null，把它右孩子指向当前根节点存起来，这样的话我们就不需要额外空间了。这样做，遍历完当前左子树，就可以回到根节点了。

当然如果当前根节点左子树为空，那么只需要保存根节点的值，然后考虑右子树即可。

总体思想就是：记当前遍历的节点为 cur。

1. cur.left 为 null，保存 cur 的值，更新 cur = cur.right

2. cur.left 不为 null，找到 cur.left 这颗子树最右边的节点记做 last

   2.1 last.right 为 null，那么将 last.right = cur，更新 cur = cur.left

   2.2 last.right 不为 null，说明之前已经访问过，第二次来到这里，表明当前子树遍历完成，保存 cur 的值，更新 cur = cur.right

![](/assets/001-100/094-s-3-1.png)  
如上图，cur 指向根节点。 当前属于 2.1 的情况，cur.left 不为 null，cur 的左子树最右边的节点的右孩子为 null，那么我们把最右边的节点的右孩子指向 cur。

![](/assets/001-100/094-s-3-2.png)  
接着，更新 cur = cur.left。

![](/assets/001-100/094-s-3-3.png)  
如上图，当前属于 2.1 的情况，cur.left 不为 null，cur 的左子树最右边的节点的右孩子为 null，那么我们把最右边的节点的右孩子指向 cur。

![](/assets/001-100/094-s-3-4.png)  
更新 cur = cur.left。

![](/assets/001-100/094-s-3-5.png)  
如上图，当前属于情况 1，cur.left 为 null，保存 cur 的值，更新 cur = cur.right。

![](/assets/001-100/094-s-3-6.png)  
如上图，当前属于 2.2 的情况，cur.left 不为 null，cur 的左子树最右边的节点的右孩子已经指向 cur，保存 cur 的值，更新 cur = cur.right。

![](/assets/001-100/094-s-3-7.png)  
如上图，当前属于情况 1，cur.left 为 null，保存 cur 的值，更新 cur = cur.right。

![](/assets/001-100/094-s-3-8.png)  
如上图，当前属于 2.2 的情况，cur.left 不为 null，cur 的左子树最右边的节点的右孩子已经指向 cur，保存 cur 的值，更新 cur = cur.right。

![](/assets/001-100/094-s-3-9.png)

当前属于情况 1，cur.left 为 null，保存 cur 的值，更新 cur = cur.right。

![](/assets/001-100/094-s-3-10.png)

cur 指向 null，结束遍历。

记当前遍历的节点为 cur。

1.cur.left 为 null，保存 cur 的值，更新 cur = cur.right

2.cur.left 不为 null，找到 cur.left 这颗子树最右边的节点记做 last

- 2.1 last.right 为 null，那么将 last.right = cur，更新 cur = cur.left

- 2.2 last.right 不为 null，说明之前已经访问过，第二次来到这里，表明当前子树遍历完成，保存 cur 的值，更新 cur = cur.right

```java
    public List<Integer> inorderTraversal2(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        TreeNode cur = root;
        while (cur != null) {
            //情况 1
            if (cur.left == null) {
                list.add(cur.val);
                cur = cur.right;
            } else {
                //找左子树最右边的节点
                TreeNode pre = cur.left;
                while (pre.right != null && pre.right != cur) {
                    pre = pre.right;
                }
                //情况 2.1
                if (pre.right == null) {
                    pre.right = cur;
                    cur = cur.left;
                }
                //情况 2.2
                if (pre.right == cur) {
                    //这里可以恢复为 null
                    pre.right = null;
                    list.add(cur.val);
                    cur = cur.right;
                }
            }
        }
        return list;
    }
```



