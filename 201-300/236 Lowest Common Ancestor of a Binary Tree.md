# 236. Lowest Common Ancestor of a Binary Tree\(M\)

[236. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

## 题目描述\(中等\)

Given a binary tree, find the lowest common ancestor \(LCA\) of two given nodes in the tree.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants \(where we allow **a node to be a descendant of itself**\).”

Given the following binary tree:  root = \[3,5,1,6,2,0,8,null,null,7,4\]

![](/assets/201-300/236-p-1.png)

Example 1:

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```

Example 2:

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
```

**Note**:

* All of the nodes' values will be unique.
* p and q are different and both values will exist in the binary tree.

## 思路

* 递归
* BFS
* 双亲节点

## 解决方法

### 递归1

两个节点p,q分为两种情况：

* p和q在相同子树中
* p和q在不同子树中

从根节点遍历，递归向左右子树查询节点信息  
递归终止条件：如果当前节点为空或等于p或q，则返回当前节点

* 递归遍历左右子树，如果左右子树查到节点都不为空，则表明p和q分别在左右子树中，因此，当前节点即为最近公共祖先；
* 如果左右子树其中一个不为空，则返回非空节点。

```java
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) {
            return null;
        }
        if (root == p || root == q) {
            return root;
        }
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left != null && right != null) {
            return root;
        } else if (left != null) {
            return left;
        } else if (right != null) {
            return right;
        } else {
            return null;
        }
    }
```

### 递归2

hasChild返回一棵树中是否包含p或者q.

在递归过程中，对于遍历到的结点进行一下判断：该点左子树是否含有p、q中的一个、右子树是否含有p、q中的一个。若是，则将此点作为结果赋值给最后结果。

```java
    TreeNode ancestor = null;

    public TreeNode lowestCommonAncestor1(TreeNode root, TreeNode p, TreeNode q) {
        hasChild(root, p, q);
        return ancestor;
    }

    public boolean hasChild(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) {
            return false;
        }
        int parent = (root == p || root == q) ? 1 : 0;
        int left = hasChild(root.left, p, q) ? 1 : 0;
        int right = hasChild(root.right, p, q) ? 1 : 0;
        int sum = parent + left + right;
        if (sum == 2) {
            ancestor = root;
        }
        return sum > 0;
    }
```

### BFS DFS

BFS遍历，DFS判断是否包含节点

层次遍历节点，调用hasChild返回以该节点为根的子树中是否同时包含p和q，若是该节点为祖先，记录当前祖先的层次，同一层次不会有第二个节点为祖先，若一层内为找到新的祖先，说明最近祖先已经找到

```java
    public TreeNode lowestCommonAncestor2(TreeNode root, TreeNode p, TreeNode q) {
        if (hasChild(p, q)) {
            return p;
        }
        if (hasChild(q, p)) {
            return q;
        }
        TreeNode ancestor = null;
        TreeNode node = null;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        int depth = 0, maxDepth = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            depth++;
            for (int i = 0; i < size; i++) {
                node = queue.poll();
                if (maxDepth == depth) {
                    //同层内不会再有别的祖先
                    continue;
                }
                if (node.left != null) {
                    queue.add(node.left);
                }
                if (node.right != null) {
                    queue.add(node.right);
                }
                if (hasChild(node, p) && hasChild(node, q)) {
                    maxDepth = depth;
                    ancestor = node;
                }
            }
            if (maxDepth < depth) {
                //遍历完该层后未发现新的祖先，说明最近祖先已经找到
                break;
            }
        }
        return ancestor;
    }

    public boolean hasChild(TreeNode root, TreeNode p) {
        if (root == null) {
            return false;
        }
        if (root == p) {
            return true;
        }
        return hasChild(root.left, p) || hasChild(root.right, p);
    }
```

### 双亲节点 DFS

DFS遍历过程中记录每个节点的双亲节点，root根节点不设置双亲节点，  
根据双亲链表找交点，可以采用双指针，链表拼接的思路来消除长度差，寻找交点，

```java
    public TreeNode lowestCommonAncestor3(TreeNode root, TreeNode p, TreeNode q) {
        Map<TreeNode, TreeNode> parent = new HashMap<>();
        dfs(root, parent);
        TreeNode l1 = p, l2 = q;
        while (l1 != l2) {
            l1 = parent.getOrDefault(l1, q);
            l2 = parent.getOrDefault(l2, p);
        }
        return l1;
    }

    public void dfs(TreeNode root, Map<TreeNode, TreeNode> parent) {
        if (root == null) {
            return;
        }
        if (root.left != null) {
            parent.put(root.left, root);
        }
        if (root.right != null) {
            parent.put(root.right, root);
        }
        dfs(root.left, parent);
        dfs(root.right, parent);
    }
```

### 双亲节点 BFS

BFS 遍历过程中记录每个节点的双亲节点，root根节点设置为空，两个哈希集来寻找两条双亲链的交点，交点即为最近祖先

```java
    public TreeNode lowestCommonAncestor4(TreeNode root, TreeNode p, TreeNode q) {
        Map<TreeNode, TreeNode> parent = new HashMap<>();
        Queue<TreeNode> queue = new LinkedList<>();
        parent.put(root, null);
        queue.add(root);
        TreeNode node = null;
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                node = queue.poll();
                if (node.left != null) {
                    queue.add(node.left);
                    parent.put(node.left, node);
                }
                if (node.right != null) {
                    queue.add(node.right);
                    parent.put(node.right, node);
                }
            }
        }
        Set<TreeNode> pAncestors = new HashSet<>();
        while (p != null) {
            pAncestors.add(p);
            p = parent.get(p);
        }
        while (!pAncestors.contains(q)) {
            q = parent.get(q);
        }
        return q;
    }
```



