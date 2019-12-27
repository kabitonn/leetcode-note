
# 507. Perfect Number(E)

[507. 完美数](https://leetcode-cn.com/problems/perfect-number/)

## 题目描述(简单)

对于一个 **正整数** ，如果它和除了它自身以外的所有正因子之和相等，我们称它为“完美数”。

给定一个 **整数** n， 如果他是完美数，返回 True，否则返回 False

 

示例：
```
输入: 28
输出: True
解释: 28 = 1 + 2 + 4 + 7 + 14
```

**提示**：
- 输入的数字 n 不会超过 100,000,000. (1e8)

## 思路

遍历因子求和，判断

## 解决方法

### 枚举

在枚举时，我们只需要从 1 到 sqrt(n) 进行枚举即可。这是因为如果 n 有一个大于 sqrt(n) 的因数 x，那么它一定有一个小于 sqrt(n) 的因数 n/x。因此我们可以从 1 到 sqrt(n) 枚举 n 的因数，当出现一个 n 的因数 x 时，我们还需要算上 n/x。此外还需要考虑特殊情况，即 x = n/x，这时我们不能重复计算。



```java
        public boolean checkPerfectNumber(int num) {
        if (num <= 1) {
            return false;
        }
        int sum = 1;
        int i = 2;
        for (; i < num / i; i++) {
            if (num % i == 0) {
                sum += i + num / i;
            }
        }
        if (i * i == num) {
            sum += i;
        }
        return sum == num;
    }
```

### 查表

完美数有限且少

```java
    public boolean checkPerfectNumber0(int num) {
        switch (num) {
            case 6:
            case 28:
            case 496:
            case 8128:
            case 33550336:
                return true;
            default:
                break;
        }
        return false;
    }
```
