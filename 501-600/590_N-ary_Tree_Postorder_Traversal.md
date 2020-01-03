
# 590. N-ary Tree Postorder Traversal(E)
 
[590. N叉树的后序遍历](https://leetcode-cn.com/problems/n-ary-tree-postorder-traversal/)

## 题目描述(简单)

给定一个 N 叉树，返回其节点值的后序遍历。

例如，给定一个 3叉树 :

![](../assets/leetcode-note/501-600/590-p-1.png)

返回其后序遍历: [5,6,3,2,4,1].

## 思路

- 递归
- 迭代

## 解决方法

### 递归

```java
    public List<Integer> postorder(Node root) {
        List<Integer> list = new LinkedList<>();
        postorder(root, list);
        return list;
    }

    public void postorder(Node p, List<Integer> list) {
        if (p == null) {
            return;
        }
        for (Node child : p.children) {
            postorder(child, list);
        }
        list.add(p.val);
    }
```

### 迭代

后序遍历顺序为：顺序子节点 根节点

其逆序为 根节点 逆序子节点

```java
    public List<Integer> postorder1(Node root) {
        List<Integer> list = new LinkedList<>();
        if (root == null) {
            return list;
        }
        Stack<Node> stack = new Stack<>();
        stack.push(root);
        Node p;
        while (!stack.isEmpty()) {
            p = stack.pop();
            list.add(0, p.val);
            for (Node child : p.children) {
                stack.push(child);
            }
        }
        return list;
    }
```