
# 508. Most Frequent Subtree Sum(M)

[508. 出现次数最多的子树元素和](https://leetcode-cn.com/problems/most-frequent-subtree-sum/)

## 题目描述(中等)

给出二叉树的根，找出出现次数最多的子树元素和。一个结点的子树元素和定义为以该结点为根的二叉树上所有结点的元素之和（包括结点本身）。然后求出出现次数最多的子树元素和。如果有多个元素出现的次数相同，返回所有出现次数最多的元素（不限顺序）。

```
示例 1
输入:

  5
 /  \
2   -3
返回 [2, -3, 4]，所有的值均只出现一次，以任意顺序返回所有值。

示例 2
输入:

  5
 /  \
2   -5
返回 [2]，只有 2 出现两次，-5 只出现 1 次。
```

**提示**： 假设任意子树元素和均可以用 32 位有符号整数表示。

## 思路

- 遍历时记录子树和更新频率，遍历完成后求最大频率子树和
- 遍历时记录最大频率的子树和

## 解决方法

### 二次遍历

```java
    public int[] findFrequentTreeSum(TreeNode root) {
        Map<Integer, Integer> map = new HashMap<>();
        sumTree(root, map);
        List<Integer> list = new ArrayList<>();
        int max = 0;
        for (int sum : map.keySet()) {
            if (map.get(sum) > max) {
                max = map.get(sum);
                list = new ArrayList<>();
                list.add(sum);
            } else if (map.get(sum) == max) {
                list.add(sum);
            }
        }
        int[] array = new int[list.size()];
        int i = 0;
        for (int n : list) {
            array[i++] = n;
        }
        return array;
    }

    public int sumTree(TreeNode p, Map<Integer, Integer> map) {
        if (p == null) {
            return 0;
        }
        int leftSum = sumTree(p.left, map);
        int rightSum = sumTree(p.right, map);
        int sum = p.val + leftSum + rightSum;
        map.put(sum, map.getOrDefault(sum, 0) + 1);
        return sum;
    }
```

### 一次遍历

```java
    Map<Integer, Integer> map = new HashMap<>();
    int maxCount = 0;
    List<Integer> list = new ArrayList<>();

    public int[] findFrequentTreeSum1(TreeNode root) {
        sumTree(root);
        int[] array = new int[list.size()];
        int i = 0;
        for (int n : list) {
            array[i++] = n;
        }
        return array;
    }

    public int sumTree(TreeNode p) {
        if (p == null) {
            return 0;
        }
        int leftSum = sumTree(p.left);
        int rightSum = sumTree(p.right);
        int sum = p.val + leftSum + rightSum;
        int frequence = map.getOrDefault(sum, 0) + 1;
        if (frequence > maxCount) {
            maxCount = frequence;
            list = new ArrayList<>();
            list.add(sum);
        } else if (frequence == maxCount) {
            list.add(sum);
        }
        map.put(sum, frequence);
        return sum;
    }
```