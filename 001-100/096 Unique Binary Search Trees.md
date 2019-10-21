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
### 递归 记忆化

```java
    public int numTrees4(int n) {
        return recursive(n, new int[n + 1]);
    }

    private int recursive(int n, int[] memo) {
        if (n == 0 || n == 1) {
            return 1;
        }
        if (memo[n] != 0) {
            return memo[n];
        }
        int sum = 0;
        for (int i = 1; i <= n; i++) {
            int leftNum = recursive(i - 1, memo);
            int rightNum = recursive(n - i, memo);
            sum += leftNum * rightNum;
        }
        memo[n] = sum;
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

trick:利用对称性，可以使得循环减少一些。
- n 是偶数的时候 1 2 | 3 4 ，for 循环中我们以每个数字为根求出每个的解。我们其实可以只求一半，根据对称性我们可以知道 1 和 4，2 和 3 求出的解分别是相等的。

- n 是奇数的时候

1 2 | 3 | 4 5，和偶数同理，只求一半，此外最中间的 3 的解也要加上

```java
    public int numTrees3(int n) {
        int[] numTrees = new int[n + 1];
        numTrees[0] = 1;
        numTrees[1] = 1;
        for (int i = 2; i <= n; i++) {
            for (int j = 1; j <= i / 2; j++) {
                numTrees[i] += numTrees[j - 1] * numTrees[i - j];
            }
            numTrees[i] *= 2;
            if ((i & 1) == 1) {
                int j = (i >> 1) + 1;
                numTrees[i] += numTrees[j - 1] * numTrees[i - j];
            }
        }
        return numTrees[n];
    }

```

### 公式

>令h ( 0 ) = 1，catalan 数满足递推式：
h ( n ) = h ( 0 ) * h ( n - 1 ) + h ( 1 ) * h ( n - 2 ) + ... + h ( n - 1 ) * h ( 0 ) ( n >=1 )
例如：h ( 2 ) = h ( 0 ) * h ( 1 ) + h ( 1 ) * h ( 0 ) = 1 * 1 + 1 * 1 = 2
h ( 3 ) = h ( 0 ) * h ( 2 ) + h ( 1 ) * h ( 1 ) + h ( 2 ) * h ( 0 ) = 1 * 2 + 1 * 1 + 2 * 1 = 5

$$ C_n = \frac{1}{n+1}C_{2n}^n= \frac{(2n)!}{(n+1)!n! }$$
$$ C_n=(2n)!/(n+1)!n!=(2n)∗(2n−1)∗...∗(n+1)/(n+1)! $$

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