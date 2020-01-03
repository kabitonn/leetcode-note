
# 561. Array Partition I(E)
 
[561. 数组拆分 I](https://leetcode-cn.com/problems/array-partition-i/)

## 题目描述(简单)

给定长度为 2n 的数组, 你的任务是将这些数分成 n 对, 例如 (a1, b1), (a2, b2), ..., (an, bn) ，使得从1 到 n 的 min(ai, bi) 总和最大。

示例 1:
```
输入: [1,4,3,2]

输出: 4
解释: n 等于 2, 最大总和为 4 = min(1, 2) + min(3, 4).
```

**提示**:
- n 是正整数,范围在 [1, 10000].
- 数组中的元素范围在 [-10000, 10000].


## 思路

最小的元素一定在最终结果里，此时考虑到和最小的元素放在一起的元素将被舍弃掉，容易知道，被舍弃的元素越小越好，则此元素应为除最小元素外其余元素中的最小值。由此，可得出第一组为（最小值，次小值）。以此类推，下一组为剩余元素中的最小值和次小值。

想要得到选出两个数最小值后,全部加起来得到最大值

所以要把两个数相差最小的为一对, 从小排到大后, 就可以得到 一个增序数组, 两两一对

## 解决方法

### 排序 拆分

```java
    public int arrayPairSum(int[] nums) {
        int n = nums.length;
        Arrays.sort(nums);
        int sum = 0;
        for (int i = 0; i < n; i += 2) {
            sum += nums[i];
        }
        return sum;
    }

```

### 桶计数 拆分

```java
    public int arrayPairSum1(int[] nums) {
        int min = -10000;
        int max = 10000;
        int[] bucket = new int[max - min + 1];
        for (int n : nums) {
            bucket[n - min]++;
        }
        int sum = 0;
        int count = nums.length;
        for (int n = bucket.length - 1; n >= 0; n--) {
            while (bucket[n] > 0) {
                bucket[n]--;
                if ((count & 1) != 0) {
                    sum += n + min;
                }
                count--;
            }
        }
        return sum;
    }
```