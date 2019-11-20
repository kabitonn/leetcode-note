
# 365. Water and Jug Problem(M)

[365. 水壶问题](https://leetcode-cn.com/problems/water-and-jug-problem/)

## 题目描述(中等)

有两个容量分别为 `x` 升和 `y` 升 的水壶以及无限多的水。请判断能否通过使用这两个水壶，从而可以得到恰好 `z`升的水？

如果可以，最后请用以上水壶中的一或两个来盛放取得的 `z` 升水。

你允许：
- 装满任意一个水壶
- 清空任意一个水壶
- 从一个水壶向另外一个水壶倒水，直到装满或者倒空

示例 1: (From the famous "Die Hard" example)
```
输入: x = 3, y = 5, z = 4
输出: True
```
示例 2:
```
输入: x = 2, y = 6, z = 5
输出: False
```

## 思路

- 规律：最大公约数
- 求所有可能解

## 解决方法

### 规律

证明 `ax + by = z` 是否有解

假设x和y的最大公约数为g
- 且 x = m * g, y = n * g; 则等式变为 z = (a * m + b * n) * g;
- 要想等式成立则z必需能被x和y的最大公约数整除。z % g = 0;

辗转相除求最大公约数

```java
    public boolean canMeasureWater(int x, int y, int z) {
        if (z == 0) {
            return true;
        }
        int r = gcd(x, y);
        if (r == 0 || x + y < z) {
            return false;
        }
        return z % r == 0;
    }

    public int gcd(int a, int b) {
        int r;
        while (b != 0) {
            r = a % b;
            a = b;
            b = r;
        }
        return a;
    }

```

### 求所有可能解

将 hold 设置为较大的桶，之后进入 while 循环，获得所有状态。
如果 hold 大于较小的桶，则 hold 减去较小桶的水量。
如果 hold 小于较小的桶，则 hold 加上最大桶的水量。

```java
    public boolean canMeasureWater1(int x, int y, int z) {
        if (z == 0 || z == x + y) {
            return true;
        }
        Set<Integer> set = new HashSet<>();
        set.add(x + y);
        if (x > y) {
            x = x ^ y;
            y = x ^ y;
            x = x ^ y;
        }
        int hold = y;
        while (!set.contains(hold)) {
            set.add(hold);
            if (hold >= x) {
                hold -= x;
            } else {
                hold += y;
            }
            if (hold == z) {
                return true;
            }
        }
        return false;
    }
```
