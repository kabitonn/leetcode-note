# 129. Sum Root to Leaf Numbers(M)


[129. 求根到叶子节点数字之和](https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/)


## 题目描述(中等)

Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path 1->2->3 which represents the number 123.

Find the total sum of all root-to-leaf numbers.

Note: A leaf is a node with no children.

Example:
```
Input: [1,2,3]
    1
   / \
  2   3
Output: 25
Explanation:
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.
Therefore, sum = 12 + 13 = 25.
```
Example 2:
```
Input: [4,9,0,5,1]
    4
   / \
  9   0
 / \
5   1
Output: 1026
Explanation:
The root-to-leaf path 4->9->5 represents the number 495.
The root-to-leaf path 4->9->1 represents the number 491.
The root-to-leaf path 4->0 represents the number 40.
Therefore, sum = 495 + 491 + 40 = 1026.
```

## 思路

- 回溯 DFS

## 解决方法



### 回溯 DFS

```java
    private int sum = 0;
    public int sumNumbers(TreeNode root) {
        List<Integer> path = new ArrayList<>();
        sumNumber(root, path);
        return sum;
    }

    private void sumNumber(TreeNode p, List<Integer> path) {
        if (p == null) {
            return;
        }
        path.add(p.val);
        if (p.left == null && p.right == null) {
            int s = 0;
            for (int n : path) {
                s *= 10;
                s += n;
            }
            sum += s;
            path.remove(path.size() - 1);
            return;
        }
        sumNumber(p.left, path);
        sumNumber(p.right, path);
        path.remove(path.size() - 1);
    }
```

### DFS

```java
    private int sum = 0;

    public int sumNumbers1(TreeNode root) {
        sumNumber1(root, 0);
        return sum;
    }

    private void sumNumber1(TreeNode p, int curSum) {
        if (p == null) {
            return;
        }
        curSum = curSum * 10 + p.val;
        if (p.left == null && p.right == null) {
            sum += curSum;
            return;
        }
        sumNumber1(p.left, curSum);
        sumNumber1(p.right, curSum);
    }

```

### 递归分治

从根节点出发经过左子树的所有路径和和从根节点出发经过右子树的所有路径和，加起来即为结果

```java
    public int sumNumbers2(TreeNode root) {
        return sumNumber2(root, 0);
    }

    private int sumNumber2(TreeNode p, int curSum) {
        if (p == null) {
            return 0;
        }
        curSum = curSum * 10 + p.val;
        if (p.left == null && p.right == null) {
            return curSum;
        }
        int sum = 0;
        sum += sumNumber2(p.left, curSum);
        sum += sumNumber2(p.right, curSum);
        return sum;
    }
```

