
# 421. Maximum XOR of Two Numbers in an Array(M)

[421. 数组中两个数的最大异或值](https://leetcode-cn.com/problems/maximum-xor-of-two-numbers-in-an-array/)

## 题目描述(中等)

给定一个非空数组，数组中元素为 $a_0, a_1, a_2, … , a_{n-1}$，其中 $0 ≤ a_i < 2^{31}$ 。

找到 $a_i$ 和 $a_j$ 最大的异或 (XOR) 运算结果，其中 `0 ≤ i,  j < n` 。

你能在O(n)的时间解决这个问题吗？

示例:
```
输入: [3, 10, 5, 25, 2, 8]

输出: 28

解释: 最大的结果是 5 ^ 25 = 28.
```

## 思路

- 暴力遍历

## 解决方法

### 暴力遍历

```java
    public int findMaximumXOR(int[] nums) {
        int n = nums.length;
        int xor = 0;
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                xor = Math.max(xor, nums[i] ^ nums[j]);
            }
        }
        return xor;
    }
```

### 前缀树Trie

0 <= a <= 2^31, 所以只需要判断30位即可

构建一个前缀树，每个节点有两个子节点，分别代表有0, 1孩子节点，每一个num按二进制形式从高位到低位插入前缀树

依次求得num和前缀树中最大的异或值，并判断大小
- 依据每一bit位上数字选择路径，若num所在节点有兄弟节点则走兄弟节点，否则，继续沿当前路径，计算当前路径上的异或值


```java
    public int findMaximumXOR1(int[] nums) {
        TrieNode tree = buildTrieTree(nums);
        int xor = 0;
        for (int num : nums) {
            xor = Math.max(xor, findMaxXOR(tree, num));
        }
        return xor;
    }

    public int findMaxXOR(TrieNode root, int num) {
        TrieNode cur = root;
        int xor = 0;
        for (int i = 30; i >= 0; i--) {
            int v = (num >>> i) & 1;
            int path = v;
            if (cur.children[1 ^ v] != null) {
                path = 1 ^ v;
            }
            cur = cur.children[path];
            xor |= (path ^ v) << i;
        }
        return xor;
    }

    public TrieNode buildTrieTree(int[] nums) {
        TrieNode root = new TrieNode();
        for (int num : nums) {
            TrieNode cur = root;
            for (int i = 30; i >= 0; i--) {
                int v = (num >>> i) & 1;
                if (cur.children[v] == null) {
                    cur.children[v] = new TrieNode();
                }
                cur = cur.children[v];
            }
            cur.val = num;
        }
        return root;
    }

    class TrieNode {
        int val;
        TrieNode[] children;

        TrieNode() {
            children = new TrieNode[2];
        }
    }
```

### 二叉树

构建一个二叉树，每个节点有两个子节点，分别代表有0, 1孩子节点，每一个num按二进制形式从高位到低位插入二叉树

双指针搜索异或节点
- 若两个指针的左右节点都不为空，则返回左右或右左的最大值(四个子节点指针都不为空)
- 否则，选择左右或右左才可能是最大值(四个子节点指针有空的)
- 否则，只能选择同一条路左左或右右(四个子节点指针不符合不同路径选法)

```java
    public int findMaximumXOR3(int[] nums) {
        TreeNode root = buildTree(nums);
        return findMaxXOR(root, root);
    }

    public int findMaxXOR(TreeNode p1, TreeNode p2) {
        while (true) {
            if (p1.left == null && p1.right == null && p2.left == null && p2.right == null) {
                break;
            }
            if (p1.left != null && p1.right != null && p2.left != null && p2.right != null) {
                return Math.max(findMaxXOR(p1.left, p2.right), findMaxXOR(p1.right, p2.left));
            }
            if (p1.left != null && p2.right != null) {
                p1 = p1.left;
                p2 = p2.right;
            } else if (p1.right != null && p2.left != null) {
                p1 = p1.right;
                p2 = p2.left;
            } else if (p1.left != null && p2.left != null) {
                p1 = p1.left;
                p2 = p2.left;
            } else if (p1.right != null && p2.right != null) {
                p1 = p1.right;
                p2 = p2.right;
            }
        }
        return p1.val ^ p2.val;
    }

    public TreeNode buildTree(int[] nums) {
        TreeNode root = new TreeNode();
        for (int num : nums) {
            TreeNode cur = root;
            for (int i = 30; i >= 0; i--) {
                if (((num >>> i) & 1) == 0) {
                    if (cur.left == null) {
                        cur.left = new TreeNode();
                    }
                    cur = cur.left;
                } else {
                    if (cur.right == null) {
                        cur.right = new TreeNode();
                    }
                    cur = cur.right;
                }
            }
            cur.val = num;
        }
        return root;
    }

    class TreeNode {
        public int val;
        public TreeNode left;
        public TreeNode right;
    }
```

### 贪心

利用性质： a ^ b = c ，则 a ^ c = b，且 b ^ c = a

如果有三个数，满足其中两个数的异或值等于另一个值，那么这三个数的顺序可以任意调换

- 二进制下，我们希望一个数尽可能大，即希望越高位上越能够出现“1”，这样这个数就是所求的最大数，这是贪心算法的思想
- 可以从最高位开始，到最低位，首先假设高位是 "1" ，把这 n 个数全部遍历一遍，看看这一位是不是真的可以是 "1" ，否则这一位就得是 "0" ，判断的依据是上面“异或运算的性质”
- 如果 a ^ b = max 成立 ，max 表示当前得到的"最大值"，那么一定有 max ^ b = a 成立。我们可以先假设当前数位上的值为 "1" ，再把当前得到的数与这个 n 个数的 前缀（因为是从高位到低位看，所以称为"前缀"）进行异或运算，放在一个哈希表中，再依次把所有前缀与这个假设的"最大值"进行异或以后得到的结果放到哈希表里查询一下，如果查得到，就说明这个数位上可以是"1"，否则就只能是 0


```java
    public int findMaximumXOR2(int[] nums) {
        int max = 0;
        int mask = 0;
        for (int i = 30; i >= 0; i--) {
            mask |= (1 << i);
            Set<Integer> set = new HashSet<>();
            for (int num : nums) {
                set.add(num & mask);
            }
            int tmp = max | (1 << i);
            for (int prefix : set) {
                if (set.contains(prefix ^ tmp)) {
                    max = tmp;
                    break;
                }
            }
        }
        return max;
    }
```