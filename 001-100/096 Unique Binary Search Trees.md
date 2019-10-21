# 096. Unique Binary Search Trees(M)

[](https://leetcode-cn.com/problems/unique-binary-search-trees/)

## 题目描述(中等)

Given n, how many structurally unique BST's (binary search trees) that store values 1 ... n?

Example:
```
Input: 3
Output: 5
Explanation:
Given n = 3, there are a total of 5 unique BST's:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3

```
## 思路

- 递归分治
- 动态规划
- 公式

## 解决方法


### 递归分治

```java
    public int numTrees2(int n) {
        return recursive(n);
    }

    private int recursive(int n) {
        if (n == 0 || n == 1) {
            return 1;
        }
        int sum = 0;
        for (int i = 1; i <= n; i++) {
            int leftNum = recursive(i - 1);
            int rightNum = recursive(n - i);
            sum += leftNum * rightNum;
        }
        return sum;
    }
```


### 动态规划

```java
    public int numTrees(int n) {
        int[] numTrees = new int[n + 1];
        numTrees[0] = 1;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                numTrees[i] += numTrees[j - 1] * numTrees[i - j];
            }
        }
        return numTrees[n];
    }

```

### 公式

```java
    public int numTrees1(int n) {
        // Note: we should use long here instead of int, otherwise overflow
        long C = 1;
        for (int i = 0; i < n; ++i) {
            C = C * 2 * (2 * i + 1) / (i + 2);
        }
        return (int) C;
    }
```