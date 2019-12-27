
# 525. Contiguous Array(M)

[525. 连续数组](https://leetcode-cn.com/problems/contiguous-array/)

## 题目描述(中等)

给定一个二进制数组, 找到含有相同数量的 0 和 1 的最长连续子数组（的长度）。

 

示例 1:
```
输入: [0,1]
输出: 2
说明: [0, 1] 是具有相同数量0和1的最长连续子数组。
```

示例 2:
```
输入: [0,1,0]
输出: 2
说明: [0, 1] (或 [1, 0]) 是具有相同数量0和1的最长连续子数组。
```

**注意**: 给定的二进制数组的长度不会超过50000。

## 思路

- 暴力遍历
- 变换为前缀和，哈希

## 解决方法

### 暴力遍历

从可能最长的连续子数组长度(len>>1<<1)开始，逐步缩短长度判断该数组内是否符合具有相同数量的0和1

```java
    public int findMaxLength(int[] nums) {
        int n = nums.length;
        int[] count = new int[n + 1];
        int zero = 0;
        for (int i = 0; i < n; i++) {
            if (nums[i] == 0) {
                zero++;
            }
            count[i + 1] = zero;
        }
        for (int len = n >> 1 << 1; len > 0; len -= 2) {
            for (int i = 0; i + len <= n; i++) {
                int j = i + len;
                if (count[j] - count[i] == len >> 1) {
                    return len;
                }
            }
        }
        return 0;
    }
```

### 前缀和

采用哈希表存储前缀和及其索引，0记为-1，1记为+1，只记录最某个前缀和第一次出现的索引，这样能保证相减获取的子数组长度尽可能长；

连续数组和为0是即为相同数量的连续子数组，比较长度取最大

```java
    public int findMaxLength1(int[] nums) {
        int n = nums.length;
        int sum = 0;
        int max = 0;
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, -1);
        for (int i = 0; i < n; i++) {
            sum += nums[i] == 0 ? -1 : 1;
            if (map.containsKey(sum)) {
                max = Math.max(max, i - map.get(sum));
            } else {
                map.put(sum, i);
            }
        }
        return max;
    }
```

采用数组模拟哈希

将[-n,n]映射为[0,2n]

```java
    public int findMaxLength2(int[] nums) {
        int n = nums.length;
        Integer[] map = new Integer[n * 2 + 1];
        int sum = 0;
        int max = 0;
        map[n] = -1;
        for (int i = 0; i < n; i++) {
            sum += nums[i] == 0 ? -1 : 1;
            if (map[n + sum] != null) {
                max = Math.max(max, i - map[n + sum]);
            } else {
                map[n + sum] = i;
            }
        }
        return max;
    }
```