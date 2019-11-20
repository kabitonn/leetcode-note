
# 449. Serialize and Deserialize BST(M)

[449. 序列化和反序列化二叉搜索树](https://leetcode-cn.com/problems/serialize-and-deserialize-bst/)

## 题目描述(中等)

序列化是将数据结构或对象转换为一系列位的过程，以便它可以存储在文件或内存缓冲区中，或通过网络连接链路传输，以便稍后在同一个或另一个计算机环境中重建。

设计一个算法来序列化和反序列化**二叉搜索树**。 对序列化/反序列化算法的工作方式没有限制。 您只需确保二叉搜索树可以序列化为字符串，并且可以将该字符串反序列化为最初的二叉搜索树。

**编码的字符串应尽可能紧凑**。

**注意**：不要使用类成员/全局/静态变量来存储状态。 你的序列化和反序列化算法应该是无状态的。


## 思路

## 解决方法

### 利用二叉树前序中序构建二叉树

二叉搜索树的中序遍历为有序序列，所以可以根据前序遍历或后序遍历经过排序得到中序遍历；

- 序列化  ：输出前序遍历序列
- 反序列化：利用前序中序遍历构建二叉树

#### 序列化 前序遍历序列

```java
    public String serialize(TreeNode root) {
        List<Integer> list = preorder(root);
        StringBuilder sb = new StringBuilder();
        for (int n : list) {
            sb.append(n).append(",");
        }
        if (list.size() != 0) {
            sb.deleteCharAt(sb.length() - 1);
        }
        return sb.toString();
    }

    public List<Integer> preorder(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        if (root == null) {
            return list;
        }
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            list.add(node.val);
            if (node.right != null) {
                stack.push(node.right);
            }
            if (node.left != null) {
                stack.push(node.left);
            }
        }
        return list;
    }

    public List<Integer> preorder_1(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        if (root == null) {
            return list;
        }
        Stack<TreeNode> stack = new Stack<>();
        TreeNode p = root;
        while (!stack.isEmpty() || p != null) {
            while (p != null) {
                list.add(p.val);
                stack.push(p);
                p = p.left;
            }
            p = stack.pop();
            p = p.right;
        }
        return list;
    }
```

#### 反序列化 前序中序构建二叉树

```java
    public TreeNode deserialize(String data) {
        if ("".equals(data)) {
            return null;
        }
        //int[] preorder = Arrays.stream(data.split(",")).mapToInt(Integer::valueOf).toArray();
        String[] strs = data.split(",");
        int[] preoder = new int[strs.length];
        int n = preoder.length;
        for (int i = 0; i < n; i++) {
            preoder[i] = Integer.valueOf(strs[i]);
        }
        int[] inorder = Arrays.copyOf(preoder, n);
        Arrays.sort(inorder);
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < n; i++) {
            map.put(inorder[i], i);
        }
        return buildTree(map, preoder, inorder, 0, n - 1, 0, n - 1);
    }

    public TreeNode buildTree(Map<Integer, Integer> map, int[] preoder, int[] inorder, int p1, int p2, int i1, int i2) {
        if (p1 > p2) {
            return null;
        }
        int val = preoder[p1];
        TreeNode node = new TreeNode(val);
        int index = map.get(val);
        int left = index - i1;
        node.left = buildTree(map, preoder, inorder, p1 + 1, p1 + left, i1, index - 1);
        node.right = buildTree(map, preoder, inorder, p1 + left + 1, p2, index + 1, i2);
        return node;
    }

```

```java
    public TreeNode deserialize_1(String data) {
        if ("".equals(data)) {
            return null;
        }
        String[] strs = data.split(",");
        int[] preoder = new int[strs.length];
        int n = preoder.length;
        for (int i = 0; i < n; i++) {
            preoder[i] = Integer.valueOf(strs[i]);
        }
        int[] inorder = Arrays.copyOf(preoder, n);
        Arrays.sort(inorder);
        return buildTree_1(preoder, inorder);
    }

    public TreeNode buildTree_1(int[] preorder, int[] inorder) {
        int n = preorder.length;
        int pre = 0, in = 0;
        Stack<TreeNode> stack = new Stack<>();
        TreeNode root = new TreeNode(preorder[pre++]);
        stack.push(root);
        for (; pre < n; pre++) {
            TreeNode curRoot = new TreeNode(preorder[pre]);
            TreeNode back = null;
            while (!stack.isEmpty() && stack.peek().val == inorder[in]) {
                back = stack.pop();
                System.out.println(back.val);
                in++;
            }
            if (back == null) {
                stack.peek().left = curRoot;
            } else {
                back.right = curRoot;
            }
            stack.push(curRoot);
        }
        return root;
    }

    public TreeNode buildTree_2(int[] preorder, int[] inorder) {
        int n = preorder.length;
        int pre = 0, in = 0;
        LinkedList<TreeNode> stack = new LinkedList<>();
        TreeNode root = new TreeNode(preorder[pre++]);
        stack.push(root);
        TreeNode curRoot = root;
        while (pre < n) {
            if (curRoot.val != inorder[in]) {
                curRoot.left = new TreeNode(preorder[pre++]);
                curRoot = curRoot.left;
            } else {
                while (!stack.isEmpty() && curRoot.val == inorder[in]) {
                    in++;
                    curRoot = stack.pop();
                }
                curRoot.right = new TreeNode(preorder[pre++]);
                curRoot = curRoot.right;
            }
            stack.push(curRoot);
        }

        return root;
    }

```

### 利用前序遍历插入构建二叉搜索树

#### 序列化 前序遍历序列

```java
    public String serialize1(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        preorder(root, sb);
        if (sb.length() != 0) {
            sb.deleteCharAt(sb.length() - 1);
        }
        return sb.toString();
    }

    public void preorder(TreeNode root, StringBuilder sb) {
        if (root == null) {
            return;
        }
        sb.append(root.val).append(",");
        preorder(root.left, sb);
        preorder(root.right, sb);
    }

```

#### 反序列化 前序遍历序列插入二叉搜索树

普通的二叉树需要两种遍历结果才能固定二叉树，而对于BST，得到BST的前序遍历，根据BST的性质，第一个元素值为根节点，小于根节点的元素为左子树，大于根节点的元素为右子树

```java
    public TreeNode deserialize1(String data) {
        if ("".equals(data)) {
            return null;
        }
        String[] strs = data.split(",");
        int[] preoder = new int[strs.length];
        int n = preoder.length;
        for (int i = 0; i < n; i++) {
            preoder[i] = Integer.valueOf(strs[i]);
        }
        int pre = 0;
        TreeNode root = new TreeNode(preoder[pre++]);
        for (; pre < n; pre++) {
            insert(root, preoder[pre]);
        }
        return root;
    }

    public void insert(TreeNode root, int val) {
        TreeNode node = new TreeNode(val);
        while (true) {
            if (root.val > val) {
                if (root.left == null) {
                    root.left = node;
                    break;
                }
                root = root.left;
            } else {
                if (root.right == null) {
                    root.right = node;
                    break;
                }
                root = root.right;
            }
        }
    }
```