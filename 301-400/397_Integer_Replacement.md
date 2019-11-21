# 397. Integer Replacement(M)

[397. 整数替换](https://leetcode-cn.com/problems/integer-replacement/)

## 题目描述(中等)

给定一个正整数 n，你可以做如下操作：

1. 如果 n 是偶数，则用 `n / 2` 替换 n。
2. 如果 n 是奇数，则可以用 `n + 1` 或 `n - 1` 替换 n。

n 变为 1 所需的最小替换次数是多少？

示例 1:
```
输入:
8

输出:
3

解释:
8 -> 4 -> 2 -> 1
```
示例 2:
```
输入:
7

输出:
4

解释:
7 -> 8 -> 4 -> 2 -> 1
或
7 -> 6 -> 3 -> 2 -> 1
```
## 思路

## 解决方法

### 递归

- 偶数  F(n/2)
- 奇数  max(F(n+1),F(n-1))

```java
    public int integerReplacement(int n) {
        if (n == 1) {
            return 0;
        }
        if ((n & 1) == 0) {
            return 1 + integerReplacement(n >>> 1);
        }
        return 1 + Math.min(integerReplacement(n + 1), integerReplacement(n - 1));
    }

```

### 递归 记忆化

```java
    public int integerReplacement1(int n) {
        return integerReplacement1(n, new HashMap<>());
    }

    public int integerReplacement1(int n, Map<Integer, Integer> memo) {
        if (n == 1) {
            return 0;
        }
        if (memo.containsKey(n)) {
            return memo.get(n);

        }
        if ((n & 1) == 0) {
            memo.put(n, 1 + integerReplacement1(n >>> 1, memo));
        } else {
            memo.put(n, 1 + Math.min(integerReplacement1(n + 1, memo), integerReplacement1(n - 1, memo)));
        }
        return memo.get(n);
    }
```

### 贪心 迭代

判断最低的两位
- 10 或 00      n/2 (去除最低位)
- 01            n-1 (后两位可变00)
- 11
  - 若 n > 3    n+1 (后两位可变00)
  - 若 n ==3    n-1 (3->2->1)

```java
    public int integerReplacement2(int n) {
        int count = 0;
        while (n != 1) {
            if ((n & 1) == 0) {
                n >>>= 1;
                count++;
            } else if ((n & 2) == 0) {
                n--;
                count++;
            } else if (n > 3) {
                n++;
                count++;
            } else {
                n--;
                count++;
            }
        }
        return count;
    }
```


