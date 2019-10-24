# 230. Kth Smallest Element in a BST(M)

[]()

## 题目描述(中等)

Given a binary search tree, write a function kthSmallest to find the **kth** smallest element in it.

**Note**:
You may assume k is always valid, 1 ≤ k ≤ BST's total elements.

Example 1:
```
Input: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
Output: 1
```
Example 2:
```
Input: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
Output: 3
```
Follow up:
What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

## 思路

二叉搜索树（二叉查找树、二叉排序树、BST）的中序遍历就是其所有节点的val按照升序排列。

- 中序遍历

## 解决方法


### 中序遍历 迭代

```java
    public int kthSmallest(TreeNode root, int k) {
        Stack<TreeNode> stack = new Stack<>();
        TreeNode p = root;
        while (!stack.isEmpty() || p != null) {
            while (p != null) {
                stack.push(p);
                p = p.left;
            }
            p = stack.pop();
            if (--k == 0) {
                return p.val;
            }
            p = p.right;
        }
        return -1;
    }

```

### 中序遍历 递归 剪枝

- 建立两个全局变量 val和 index ，分别用于存储答案与计数：
    - 在每次访问节点时，执行 index +1 ；
    - 当index == k 时，代表已经到达第 k 个节点，此时记录答案至val；
- 找到答案后，已经不用继续遍历，因此每次判断index>=k,若大于说明第k个已经访问了。



```java
    int index = 0;
    int val = -1;

    public int kthSmallest1(TreeNode root, int k) {
        inorderTraversal(root, k);
        return val;
    }


    public void inorderTraversal(TreeNode root, int k) {
        if (root == null || index >= k) {
            return;
        }
        inorderTraversal(root.left, k);
        if (++index == k) {
            val = root.val;
            return;
        }
        inorderTraversal(root.right, k);
    }

```