# 303. Range Sum Query - Immutable(E)
[303. Range Sum Query - Immutable](https://leetcode-cn.com/problems/range-sum-query-immutable/)

## 题目描述\(简单\)

给定一个整数数组  nums，求出数组从索引 i 到 j  (i ≤ j) 范围内元素的总和，包含 i,  j 两点。

示例：
```
给定 nums = [-2, 0, 3, -5, 2, -1]，求和函数为 sumRange()

sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3
```

**说明**:

- 你可以假设数组不可变。
- 会多次调用 sumRange 方法。


## 思路

## 解决方法

### 暴力法

```java
public class NumArray {
    private int[] nums;
    public NumArray(int[] nums) {
        this.nums = nums;
    }

    public int sumRange(int i, int j) {
        int sum = 0;
        for(;i<=j&&i<nums.length;i++) {
            sum+=nums[i];
        }
        return sum;
    }
}
```
时间复杂度：每次查询的时间 O(n)，每个 sumrange 查询需要 O(n) 时间。
空间复杂度：O(1)


### 动态规划+缓存


$sums[k]$ 定义为 $nums[0 \cdots k-1]$的累积和

$sumrange(i, j)=sums[j+1] - sums[i]$


```java
public class NumArray {
    private int[] sums;
    public NumArray(int[] nums) {
        this.sums = new int[nums.length+1];
        for(int i=0;i<nums.length;i++) {
            sums[i+1] = sums[i]+nums[i];
        }
    }
    public int sumRange(int i, int j) {
        return sums[j+1]-sums[i];
    }
}
```
时间复杂度：每次查询的时间 O(1)，O(n)预计算时间。由于累积和被缓存，每个sumrange查询都可以用 O(1) 时间计算。
空间复杂度：O(n).





