
# 589. N-ary Tree Preorder Traversal(E)
 
[589. N叉树的前序遍历](https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/)

## 题目描述(简单)

给定一个 N 叉树，返回其节点值的前序遍历。

例如，给定一个 3叉树 :

![](../assets/leetcode-note/501-600/589-p-1.png)

返回其前序遍历: [1,3,5,6,2,4]。

## 思路

- 递归
- 迭代

## 解决方法

### 递归

```java
    public List<Integer> preorder(Node root) {
        List<Integer> list = new LinkedList<>();
        preorder(root, list);
        return list;
    }

    public void preorder(Node p, List<Integer> list) {
        if (p == null) {
            return;
        }
        list.add(p.val);
        for (Node child : p.children) {
            preorder(child, list);
        }
    }

```

### 迭代

前序遍历为 根节点 顺序子节点

```java
    public List<Integer> preorder1(Node root) {
        List<Integer> list = new LinkedList<>();
        if (root == null) {
            return list;
        }
        Stack<Node> stack = new Stack<>();
        stack.push(root);
        Node p;
        while (!stack.isEmpty()) {
            p = stack.pop();
            list.add(p.val);
            List<Node> nodes = p.children;
            for (int i = nodes.size() - 1; i >= 0; i--) {
                stack.push(nodes.get(i));
            }
        }
        return list;
    }
```