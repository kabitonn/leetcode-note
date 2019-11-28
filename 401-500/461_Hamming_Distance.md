
# 461. Hamming Distance(E)

[461. 汉明距离](https://leetcode-cn.com/problems/hamming-distance/)

## 题目描述(中等)

两个整数之间的汉明距离指的是这两个数字对应二进制位不同的位置的数目。

给出两个整数 x 和 y，计算它们之间的汉明距离。

**注意：**
0 ≤ x, y < 2^31.

示例:
```
输入: x = 1, y = 4

输出: 2

解释:
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑

上面的箭头指出了对应二进制位不同的位置。
```

## 思路

- 逐位判断异或值
- 判断异或值的距离

## 解决方法

### 逐位判断异或值

按位计算异或，然后同时将两个数右移一位

```java
    public int hammingDistance(int x, int y) {
        int distance = 0;
        for (int i = 0; i < 31; i++) {
            distance += (x & 1) ^ (y & 1);
            x >>= 1;
            y >>= 1;
        }
        return distance;
    }

```

### 判断异或值的距离

先将两个数异或运算得到n，那么n里面1的个数就是结果，

```java
    public int hammingDistance1(int x, int y) {
        int xor = x ^ y;
        int distance = 0;
        while (xor != 0) {
            distance++;
            xor = xor & (xor - 1);
        }
        return distance;
    }

    public int hammingDistance2(int x, int y) {
        return Integer.bitCount(x ^ y);
    }
```