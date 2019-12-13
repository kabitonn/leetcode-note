
# 477. Total Hamming Distance(M)

[477. 汉明距离总和](https://leetcode-cn.com/problems/total-hamming-distance/)

## 题目描述(中等)

两个整数的 汉明距离 指的是这两个数字的二进制数对应位不同的数量。

计算一个数组中，任意两个数之间汉明距离的总和。

示例:
```
输入: 4, 14, 2

输出: 6

解释: 在二进制表示中，4表示为0100，14表示为1110，2表示为0010。（这样表示是为了体现后四位之间关系）
所以答案为：
HammingDistance(4, 14) + HammingDistance(4, 2) + HammingDistance(14, 2) 
= 2 + 2 + 2 = 6.
```

**注意**:
- 数组中元素的范围为从 0到 10^9。
- 数组的长度不超过 10^4。

## 思路

- 遍历
- 统计每一位上1和0的个数，海明距离为个数乘积

## 解决方法

### 遍历任意两数海明距离求和

!> 超时

```java
    public int totalHammingDistance(int[] nums) {
        int n = nums.length;
        int sum = 0;
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                sum += Integer.bitCount(nums[i] ^ nums[j]);
            }
        }
        return sum;
    }
```

### 统计每位1和0个数

汉明距离等于两个数二进制表示中对应位置不同的数量。

要计算数组中任意两个数的汉明距离的总和，可以先算出数组中任意两个数二进制第 i 位的汉明距离的总和，在将所有的 k 位之和相加。也就是说，二进制中的每一位都是可以独立计算的。

因此，考虑数组中每个数二进制的第 i 位，假设一共有 t 个 0 和 n - t 个 1，那么显然在第 i 位的汉明距离的总和为 t * (n - t)。

```java
    public int totalHammingDistance1(int[] nums) {
        int sum = 0;
        for (int i = 0; i < 32; i++) {
            int[] count = new int[2];
            int s = 0;
            for (int num : nums) {
                int tmp = num >> i;
                count[(tmp & 1)]++;
                s += tmp;
            }
            if (s == 0) {
                break;
            }
            sum += count[0] * count[1];
        }
        return sum;
    }
```

```java
    public int totalHammingDistance2(int[] nums) {
        int sum = 0;
        int[] countOne = new int[32];
        for (int num : nums) {
            for (int i = 0; i < 32; i++) {
                countOne[i] += num & 1;
                num >>= 1;
            }
        }
        for (int i = 0; i < 32; i++) {
            sum += (nums.length - countOne[i]) * countOne[i];
        }
        return sum;
    } 
```

时间复杂度：O(NlogC)，其中 C 是常数，表示数组中数可能的最大值。

空间复杂度：O(logC)，也可以优化到 O(1)，但可能会减少缓存命中，从而略微增加运行时间。


