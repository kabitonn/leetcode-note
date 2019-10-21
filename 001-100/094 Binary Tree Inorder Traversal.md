# 094. Binary Tree Inorder Traversal(M)
[094. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)


## 题目描述(中等)

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

- 递归
- 迭代

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
时间复杂度：O(n)。

空间复杂度：O(h)，栈消耗，h 是二叉树的高度。


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

时间复杂度：O(n)。

空间复杂度：O(h)，栈消耗，h 是二叉树的高度。
