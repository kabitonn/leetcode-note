
# 338. Counting Bits(M)

[338. 比特位计数](https://leetcode-cn.com/problems/counting-bits/)

## 题目描述(中等)

给定一个非负整数 num。对于 `0 ≤ i ≤ num` 范围中的每个数字 i ，计算其二进制数中的 1 的数目并将它们作为数组返回。

示例 1:
```
输入: 2
输出: [0,1,1]
```
示例 2:
```
输入: 5
输出: [0,1,1,2,1,2]
```
**进阶**:
- 给出时间复杂度为O(n*sizeof(integer))的解答非常容易。但你可以在线性时间O(n)内用一趟扫描做到吗？
- 要求算法的空间复杂度为O(n)。
- 你能进一步完善解法吗？要求在C++或任何其他语言中不使用任何内置函数（如 C++ 中的 __builtin_popcount）来执行此操作。



## 思路

- 对一个数字解决问题，并应用到全部
- 利用已有的计数结果来生成新的计数结果

## 解决方法

### 循环统计每个数中1的个数

`n & (n - 1)`得到最低位的1置0的结果

```java
    public int[] countBits(int num) {
        int[] bits = new int[num + 1];
        for (int i = 0; i <= num; i++) {
            int n = i;
            while (n != 0) {
                n = n & (n - 1);
                bits[i]++;
            }
        }
        return bits;
    }
```

### 动态规划 + 最后设置位

最后设置位是从右到左第一个为1的位

`P(x)=P(x&(x−1))+1;`

```java
    public int[] countBits1(int num) {
        int[] bits = new int[num + 1];
        for (int i = 1; i <= num; i++) {
            bits[i] = 1 + bits[i & (i - 1)];
        }
        return bits;
    }
```

### 动态规划 + 最低有效位

`P(x)=P(x/2)+(x%2)`

```java
    public int[] countBits2(int num) {
        int[] bits = new int[num + 1];
        for (int i = 1; i <= num; i++) {
            bits[i] = bits[i >> 1] + (i & 1);
        }
        return bits;
    }
```
### 动态规划 + 最高有效位

`P(x+b)=P(x)+1,b=2^m>x`


```java
    public int[] countBits3(int num) {
        int[] bits = new int[num + 1];
        int n = 1, i = 0;
        while (n <= num) {
            while (i < n && i + n <= num) {
                bits[i + n] = bits[i] + 1;
                i++;
            }
            i = 0;
            n <<= 1;
        }
        return bits;
    }
```