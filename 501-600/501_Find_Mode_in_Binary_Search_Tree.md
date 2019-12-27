
# 501. Find Mode in Binary Search Tree(E)

[501. 二叉搜索树中的众数](https://leetcode-cn.com/problems/find-mode-in-binary-search-tree/)

## 题目描述(简单)

给定一个有相同值的二叉搜索树（BST），找出 BST 中的所有众数（出现频率最高的元素）。

假定 BST 有如下定义：

- 结点左子树中所含结点的值小于等于当前结点的值
- 结点右子树中所含结点的值大于等于当前结点的值
- 左子树和右子树都是二叉搜索树

```
例如：
给定 BST [1,null,2,2],

   1
    \
     2
    /
   2
返回[2].
```

**提示**：如果众数超过1个，不需考虑输出顺序

**进阶**：你可以不使用额外的空间吗？（假设由递归产生的隐式调用栈的开销不被计算在内）

## 思路

- 中序后遍历计数
- 中序时遍历计数


## 解决方法

### 迭代中序后 遍历计数

中序遍历序列中，统计各个频率的数值

```java
    //中序+遍历计数
    public int[] findMode0(TreeNode root) {
        if (root == null) {
            return new int[]{};
        }
        List<Integer> list = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode p = root;
        while (p != null || !stack.isEmpty()) {
            while (p != null) {
                stack.push(p);
                p = p.left;
            }
            p = stack.pop();
            list.add(p.val);
            p = p.right;
        }
        Map<Integer, List<Integer>> map = new HashMap<>();
        int count = 0;
        int max = 0;
        for (int i = 0; i < list.size(); i++) {
            if (i == 0) {
                count = 1;
            }
            else if (list.get(i).equals(list.get(i - 1))) {
                count++;
            } else {
                if (!map.containsKey(count)) {
                    map.put(count, new ArrayList<>());
                }
                map.get(count).add(list.get(i - 1));
                max = Math.max(max, count);
                count = 1;
            }
        }
        if (!map.containsKey(count)) {
            map.put(count, new ArrayList<>());
        }
        map.get(count).add(list.get(list.size() - 1));
        max = Math.max(max, count);
        int[] mode = new int[map.get(max).size()];
        int i = 0;
        for (int n : map.get(max)) {
            mode[i++] = n;
        }
        return mode;
    }
```

### 递归中序后 遍历 

中序遍历序列中，若频率若大于当前最大频率，清空或新建List

```java
    public int[] findMode1(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        inorder(root, list);
        List<Integer> modeList = new ArrayList<>();
        Integer prev = null;
        int count = 0, max = 0;
        for (int n : list) {
            if (prev == null || n != prev) {
                count = 1;
                prev = n;
            } else {
                count++;
            }
            if (count > max) {
                max = count;
                modeList = new ArrayList<>();
                modeList.add(prev);
            } else if (count == max) {
                modeList.add(prev);
            }
        }
        int[] mode = new int[modeList.size()];
        int i = 0;
        for (int n : modeList) {
            mode[i++] = n;
        }
        return mode;
    }

    public void inorder(TreeNode p, List<Integer> list) {
        if (p == null) {
            return;
        }
        inorder(p.left, list);
        list.add(p.val);
        inorder(p.right, list);
    }
```

### 迭代中序时 遍历

中序遍历过程中，频率若大于当前最大频率，清空或新建List

```java
    public int[] findMode(TreeNode root) {
        if (root == null) {
            return new int[]{};
        }
        List<Integer> list = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode p = root;
        Integer prev = null;
        int count = 0, max = 0;
        while (p != null || !stack.isEmpty()) {
            while (p != null) {
                stack.push(p);
                p = p.left;
            }
            p = stack.pop();
            if (prev == null || p.val != prev) {
                count = 1;
                prev = p.val;
            } else {
                count++;
            }
            if (count > max) {
                max = count;
                list = new ArrayList<>();
                list.add(prev);
            } else if (count == max) {
                list.add(prev);
            }
            p = p.right;
        }
        int[] mode = new int[list.size()];
        int i = 0;
        for (int n : list) {
            mode[i++] = n;
        }
        return mode;
    }

```

### 递归中序时 遍历

利用全局变量 在递归中统计频率

```java
    Integer prev = null;
    int count = 0, maxCount = 0;
    List<Integer> list = new ArrayList<>();

    public int[] findMode2(TreeNode root) {
        inorder(root);
        int[] mode = new int[list.size()];
        int i = 0;
        for (int n : list) {
            mode[i++] = n;
        }
        return mode;
    }

    public void inorder(TreeNode p) {
        if (p == null) {
            return;
        }
        inorder(p.left);
        if (prev == null || p.val != prev) {
            count = 1;
            prev = p.val;
        } else {
            count++;
        }
        if (count > maxCount) {
            maxCount = count;
            list = new ArrayList<>();
            list.add(prev);
        } else if (count == maxCount) {
            list.add(prev);
        }
        inorder(p.right);
    }
```
