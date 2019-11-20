# 331. Verify Preorder Serialization of a Binary Tree(M)

[331. 验证二叉树的前序序列化](https://leetcode-cn.com/problems/verify-preorder-serialization-of-a-binary-tree/)

## 题目描述(中等)

序列化二叉树的一种方法是使用前序遍历。当我们遇到一个非空节点时，我们可以记录下这个节点的值。如果它是一个空节点，我们可以使用一个标记值记录，例如 #。
```
     _9_
    /   \
   3     2
  / \   / \
 4   1  #  6
/ \ / \   / \
# # # #   # #
```
例如，上面的二叉树可以被序列化为字符串 `"9,3,4,#,#,1,#,#,2,#,6,#,#"`，其中 # 代表一个空节点。

给定一串以逗号分隔的序列，验证它是否是正确的二叉树的前序序列化。编写一个在不重构树的条件下的可行算法。

每个以逗号分隔的字符或为一个整数或为一个表示 `null` 指针的 `'#'` 。

你可以认为输入格式总是有效的，例如它永远不会包含两个连续的逗号，比如 `"1,,3"` 。

示例 1:
```
输入: "9,3,4,#,#,1,#,#,2,#,6,#,#"
输出: true
```
示例 2:
```
输入: "1,#"
输出: false
```
示例 3:
```
输入: "9,#,#,1"
输出: false
```

## 思路

- 模拟二叉树前序遍历过程
- 模拟建树
- 二叉树性质
- 正则匹配替换

## 解决方法

### 模拟二叉树前序遍历过程

模拟过程中，当前节点的子节点都已访问或者当前节点为空节点，节点出栈，出栈时修改栈顶节点即父节点的子节点已访问数目；否则入栈

```java
     public boolean isValidSerialization(String preorder) {
        String[] strs = preorder.split(",");
        int n = strs.length;
        if (n == 0) {
            return false;
        }
        int[] visited = new int[n];
        int[] stack = new int[n + 1];
        int top = 0;
        int index = 0;
        stack[top++] = index++;
        while (index <= n) {
            int node = stack[top - 1];
            if ("#".equals(strs[node]) || visited[node] == 2) {
                if (--top == 0) {
                    break;
                }
                visited[stack[top - 1]]++;
            } else {
                stack[top++] = index++;
            }

        }
        return index == n;
    }
```

### 模拟前序遍历递归建树过程

当前元素为空节点且栈顶为空节点，弹出空节点和父节点后入栈相当于父节点和两个空的子节点替换为空节点

```java
     public boolean isValidSerialization_0(String preorder) {
        String[] strs = preorder.split(",");
        int n = strs.length;
        if (n == 0) {
            return false;
        }
        String[] stack = new String[n + 1];
        int top = 0;
        for (String s : strs) {
            while (top > 0 && "#".equals(stack[top - 1]) && "#".equals(s)) {
                if (--top == 0) {
                    return false;
                }
                top--;
            }
            stack[top++] = s;
        }
        return top == 1 && "#".equals(stack[top - 1]);
    }
```

### 二叉树性质

- 空节点当作树的叶子节点。
- 树的特征：当遍历完整棵二叉树时，叶子节点的数目总是比非叶子节点的数目多1。
从数组开头遍历，在遍历的过程中判断应保证非叶子节点的数目 `nodes` 不小于叶子节点的数目`leaves`
- 当遍历到最后一个位置时，需要保证最后一个元素为空，否则，该数组不可能是一个二叉树的前序序列化。
- 另外，叶节点的数目不可能小于2。

```java
     public boolean isValidSerialization2(String preorder) {
        String[] strs = preorder.split(",");
        int n = strs.length;
        if (n == 0) {
            return false;
        }
        int leaves = 0;
        int nodes = 0;
        int i = 0;
        for (String s : strs) {
            if ("#".equals(s)) {
                leaves++;
            } else {
                nodes++;
            }
            i++;
            if (leaves > nodes + 1 || (i < n && leaves == nodes + 1)) {
                break;
            }
        }
        if (i == n && leaves == nodes + 1) {
            return true;
        } else {
            return false;
        }
    }
```

### 正则匹配替换

- 前序序列化二叉树, 叶子节点一定是序列化字符串中形如"1,#,#"的子串.
- 把全部叶子摘掉 ( 即把形如"1,#,#"的子串, 替换为"#" ). 摘掉叶子后, 叶子节点的父节点变为新的叶子节点. 迭代此过程直到只剩根节点或者不存在有效叶子节点.
- "#".equals(preorder) 判断是否剩下根节点

```java
     static final Pattern pattern = Pattern.compile("[^,#]+,#,#");

    public boolean isValidSerialization1(String preorder) {
        Matcher matcher = pattern.matcher(preorder);
        while (!"#".equals(preorder) && matcher.find()) {
            preorder = matcher.replaceAll("#");
            matcher = pattern.matcher(preorder);
        }
        return "#".equals(preorder);
    }
```