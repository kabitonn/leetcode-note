# 105. Construct Binary Tree from Preorder and Inorder Traversal (M)


[105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)


## 题目描述(中等)

Given preorder and inorder traversal of a tree, construct the binary tree.

**Note**:
You may assume that duplicates do not exist in the tree.

For example, given
```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
Return the following binary tree:

    3
   / \
  9  20
    /  \
   15   7
```


## 思路



- 递归
- 迭代


## 解决方法



### 递归

先序遍历的顺序是根节点，左子树，右子树。中序遍历的顺序是左子树，根节点，右子树。
只需要根据先序遍历得到根节点，然后在中序遍历中找到根节点的位置，它的左边就是左子树的节点，右边就是右子树的节点。



```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
首先根据 preorder 找到根节点是 3

然后根据根节点将 inorder 分成左子树和右子树
左子树
inorder [9]

右子树
inorder [12,20,7]

把相应的前序遍历的数组也加进来
左子树
preorder[9] 
inorder [9]

右子树
preorder[20 15 7] 
inorder [12,20,7]
```

只需要构造左子树和右子树即可，成功把大问题化成了小问题
然后重复上边的步骤继续划分，直到 preorder 和 inorder 都为空，返回 null 即可

```java
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        int n = inorder.length;
        return buildTree(preorder, 0, n - 1, inorder, 0, n - 1);
    }

    private TreeNode buildTree(int[] preorder, int preL, int preR, int[] inorder, int inL, int inR) {
        if (preL > preR) {
            return null;
        }
        int rootVal = preorder[preL];
        TreeNode rootNode = new TreeNode(rootVal);
        int index = inL;
        for (; index <= inR; index++) {
            if (inorder[index] == rootVal) {
                break;
            }
        }
        int leftNum = index - inL;
        rootNode.left = buildTree(preorder, preL + 1, preL + leftNum, inorder, inL, index - 1);
        rootNode.right = buildTree(preorder, preL + leftNum + 1, preR, inorder, index + 1, inR);
        return rootNode;
    }
```
在中序遍历中找到根节点的位置每次都得遍历中序遍历的数组去寻找，以用一个HashMap把中序遍历数组的每个元素的值和下标存起来，这样寻找根节点的位置就可以直接得到了

```java
    public TreeNode buildTree1(int[] preorder, int[] inorder) {
        int n = inorder.length;
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < n; i++) {
            map.put(inorder[i], i);
        }
        return buildTree(map, preorder, 0, n - 1, inorder, 0, n - 1);
    }

    private TreeNode buildTree(Map<Integer, Integer> map, int[] preorder, int preL, int preR, int[] inorder, int inL, int inR) {
        if (preL > preR) {
            return null;
        }
        int rootVal = preorder[preL];
        TreeNode rootNode = new TreeNode(rootVal);
        int index = map.get(rootVal);
        int leftNum = index - inL;
        rootNode.left = buildTree(map, preorder, preL + 1, preL + leftNum, inorder, inL, index - 1);
        rootNode.right = buildTree(map, preorder, preL + leftNum + 1, preR, inorder, index + 1, inR);
        return rootNode;
    }
```

### 递归2

用pre变量保存当前要构造的树的根节点，从根节点开始递归的构造左子树和右子树，in变量指向当前根节点可用数字的开头，然后对于当前pre有一个停止点stop，从in到stop表示要构造的树当前的数字范围。

```
      3
    /   \
   9     7
  / \
 20  15

前序遍历数组和中序遍历数组
preorder = [ 3, 9, 20, 15, 7 ]
inorder = [ 20, 9, 15, 3, 7 ]   
p 代表 pre，i 代表 in，s 代表 stop

首先构造根节点为 3 的树，可用数字是 i 到 s
s 初始化一个树中所有的数字都不会相等的数，所以代码中用了一个 long 来表示
3, 9, 20, 15, 7 
^  
p
20, 9, 15, 3, 7
^              ^
i              s

考虑根节点为 3 的左子树，                考虑根节点为 3 的树的右子树，
stop 值是当前根节点的值 3                只知道 stop 值是上次的 s
新的根节点是 9,可用数字是 i 到 s 
不包括 s
3, 9, 20, 15, 7                       3, 9, 20, 15, 7                
   ^                                    
   p
20, 9, 15, 3, 7                       20, 9, 15, 3, 7                     
^          ^                                         ^
i          s                                         s

递归出口的情况
3, 9, 20, 15, 7 
       ^  
       p
20, 9, 15, 3, 7
^    
i   
s   
此时 in 和 stop 相等，表明没有可用的数字，所以返回 null，并且表明此时到达了某个树的根节点，所以 i 后移。

```

```java
    public TreeNode buildTree2(int[] preorder, int[] inorder) {
        return buildTree(preorder, inorder, (long) Integer.MAX_VALUE + 1);
    }

    int pre = 0;
    int in = 0;

    private TreeNode buildTree(int[] preorder, int[] inorder, long stop) {
        //到达末尾返回 null
        if (pre == preorder.length) {
            return null;
        }
        //到达停止点返回 null
        //当前停止点已经用了，in 后移
        if (inorder[in] == stop) {
            in++;
            return null;
        }
        int rootVal = preorder[pre++];
        TreeNode root = new TreeNode(rootVal);
        //左子树的停止点是当前的根节点
        root.left = buildTree(preorder, inorder, rootVal);
        //右子树的停止点是当前树的停止点(上层根节点)
        root.right = buildTree(preorder, inorder, stop);
        return root;
    }
```

### 迭代

假设我们要还原的树是下图
```
      3
    /   \
   9     7
  / \
 20  15
```
首先假设我们只有先序遍历的数组，如果还原一颗树，会遇到什么问题。
```
preorder = [3, 9, 20, 15, 7 ]
```
首先我们把 3 作为根节点，然后到了 9 ，就出现一个问题，9 是左子树还是右子树呢？

所以需要再加上中序遍历的数组来确定。
```
inorder = [ 20, 9, 15, 3, 7 ]
```
我们知道中序遍历，首先遍历左子树，然后是根节点，最后是右子树。这里第一个遍历的是 20 ，说明先序遍历的 9 一定是左子树，利用反证法证明。

假如 9 是右子树，根据先序遍历 preorder = [ 3, 9, 20, 15, 7 ]，说明根节点 3 的左子树是空的，

左子树为空，那么中序遍历就会先遍历根节点 3，而此时是 20，假设不成立，说明 9 是左子树。

接下来的 20 同理，所以可以目前构建出来的树如下。
```
      3
    /   
   9    
  / 
 20
```
同时，还注意到此时先序遍历的 20 和中序遍历 20 相等了，说明什么呢？

说明中序遍历的下一个数 15 不是左子树了，如果是左子树，那么中序遍历的第一个数就不会是 20。

所以 15 一定是右子树了，现在还有个问题，它是 20 的右子树，还是 9 的右子树，还是 3 的右子树？

我们来假设几种情况，来想一下。

如果是 3 的右子树， 20 和 9 的右子树为空，那么中序遍历就是20 9 3 15。

如果是 9 的右子树，20 的右子树为空，那么中序遍历就是20 9 15。

如果是 20 的右子树，那么中序遍历就是20 15。

之前已经遍历的根节点是 3 9 20，把它倒过来,即20 9 3，然后和上边的三种中序遍历比较，会发现 15 就是最后一次相等的节点的右子树。

第 1 种情况，中序遍历是20 9 3 15，和20 9 3 都相等，所以 15 是3 的右子树。

第 2 种情况，中序遍历是20 9 15，只有20 9 相等，所以 15 是 9 的右子树。

第 3 种情况，中序遍历就是20 15，只有20 相等，所以 20 是 15 的右子树。

而此时我们的中序遍历数组是inorder = [ 20, 9 ,15, 3, 7 ]，20 匹配，9匹配，最后一次匹配是 9，所以 15 是 9的右子树。
```
     3
    /   
   9    
  / \
 20  15
```
综上所述，用一个栈保存已经遍历过的节点，遍历前序遍历的数组，一直作为当前根节点的左子树，直到当前节点和中序遍历的数组的节点相等了，那么我们正序遍历中序遍历的数组，倒着遍历已经遍历过的根节点（用栈的 pop 实现），找到最后一次相等的位置，把它作为该节点的右子树。

当前指向的curRoot(栈顶节点)和中序遍历的节点相等，说明该节点左子树已访问过(即已经生成)，新生成的根节点为左子树已经访问过的根节点的右子树。


```java
    public TreeNode buildTree3(int[] preorder, int[] inorder) {
        int n = preorder.length;
        if (n == 0) {
            return null;
        }
        int pre = 0, in = 0;
        Stack<TreeNode> stack = new Stack<>();
        int rootVal = preorder[pre++];
        TreeNode root = new TreeNode(rootVal);
        TreeNode curRoot = root;
        stack.push(curRoot);
        while (pre < n) {
            if (curRoot.val != inorder[in]) {
                curRoot.left = new TreeNode(preorder[pre++]);
                curRoot = curRoot.left;
            } else {
                while (!stack.isEmpty() && stack.peek().val == inorder[in]) {
                    curRoot = stack.pop();
                    in++;
                }
                curRoot.right = new TreeNode(preorder[pre++]);
                curRoot = curRoot.right;
            }
            stack.push(curRoot);
        }
        return root;
    }
```


### 迭代2

栈顶节点和中序遍历的节点相等，说明该节点左子树已访问过(即已经生成)，新生成的根节点为左子树已经访问过的根节点的右子树。

原理和上述迭代一样

```java
    public TreeNode buildTree4(int[] preorder, int[] inorder) {
        int n = preorder.length;
        if (n == 0) {
            return null;
        }
        int pre = 0, in = 0;
        Stack<TreeNode> stack = new Stack<>();
        int rootVal = preorder[pre++];
        TreeNode root = new TreeNode(rootVal);
        stack.push(root);
        for (; pre < n; pre++) {
            TreeNode curRoot = new TreeNode(preorder[pre]);
            TreeNode back = null;
            while (!stack.isEmpty() && stack.peek().val == inorder[in]) {
                back = stack.pop();
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
```