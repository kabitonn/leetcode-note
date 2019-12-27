
# 523. Continuous Subarray Sum(M)

[523. 连续的子数组和](https://leetcode-cn.com/problems/continuous-subarray-sum/)

## 题目描述(中等)

给定一个包含非负数的数组和一个目标整数 k，编写一个函数来判断该数组是否含有连续的子数组，其大小至少为 2，总和为 k 的倍数，即总和为 n*k，其中 n 也是一个整数。

示例 1:
```
输入: [23,2,4,6,7], k = 6
输出: True
解释: [2,4] 是一个大小为 2 的子数组，并且和为 6。
```

示例 2:
```
输入: [23,2,6,4,7], k = 6
输出: True
解释: [23,2,6,4,7]是大小为 5 的子数组，并且和为 42。
```

**说明**:
- 数组的长度不会超过10,000。
- 你可以认为所有数字总和在 32 位有符号整数范围内。


## 思路

- 暴力遍历
- 同余 哈希

## 解决方法

### 暴力遍历

```java
    public boolean checkSubarraySum(int[] nums, int k) {
        int n = nums.length;
        if (k == 0) {
            for (int i = 0; i < n - 1; i++) {
                if (nums[i] == 0 && nums[i + 1] == 0) {
                    return true;
                }
            }
            return false;
        }
        for (int i = 0; i < n - 1; i++) {
            int sum = nums[i];
            for (int j = i + 1; j < n; j++) {
                sum += nums[j];
                if (sum % k == 0) {
                    return true;
                }
            }
        }
        return false;
    }
```

```java
    public boolean checkSubarraySum0(int[] nums, int k) {
        int n = nums.length;
        int[] sums = new int[n + 1];
        for (int i = 0; i < n; i++) {
            sums[i + 1] = sums[i] + nums[i];
        }
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 2; j <= n; j++) {
                int sum = sums[j] - sums[i];
                if (k == 0) {
                    if (sum == 0) {
                        return true;
                    }
                } else if (sum % k == 0) {
                    return true;
                }
            }
        }
        return false;
    }
```

### 同余 哈希

- 要判断的是 $(sum[j] - sum[i])\%k(sum[j]−sum[i])%k$ 是否等于 0。
- 根据 mod 运算的性质，我们知道 $(sum[j] - sum[i])\%k = sum[j]\%k - sum[i]\%k$。
- 故若想 $(sum[j] - sum[i])\%k = 0(sum[j]−sum[i])%k=0$，则必有 $sum[j]\%k = sum[i]\%k$。


使用 HashMap 来保存到第 i 个元素为止的累积和，对这个前缀和除以 k 取余数

遍历一遍给定的数组，记录到当前位置为止的 sum%k 。一旦我们找到新的 sum%k 的值（即在 HashMap 中没有这个值），我们就往 HashMap 中插入一条记录 (sum%k, i)。

基于这一观察，得出结论：无论何时，只要 sum%k 的值已经被放入 HashMap 中了，代表着有两个索引 i 和 j ，它们之间元素的和是 k 的整数倍。因此，只要 HashMap 中有相同的 sum%k ，我们就可以直接返回 true 。


```java
    public boolean checkSubarraySum1(int[] nums, int k) {
        int n = nums.length;
        Map<Integer, Integer> sumMap = new HashMap<>();
        sumMap.put(0, -1);
        int sum = 0;
        for (int i = 0; i < n; i++) {
            sum += nums[i];
            if (k != 0) {
                sum %= k;
            }
            if (sumMap.containsKey(sum)) {
                if (i - sumMap.get(sum) > 1) {
                    return true;
                }
            } else {
                sumMap.put(sum, i);
            }
        }
        return false;
    }
```

时间复杂度： O(n) 。仅需要遍历 nums 数组一遍。
空间复杂度： O(min(n,k)) 。 HashMap 最多包含 min(n,k) 个不同的元素。


- 每当我们计算出一个前缀和 sum[j] 时，我们判断哈希表中是否存在键值为 $sum[j]\%k$，若存在则有 $sum[j]\%k=sum[i]\%k$，我们返回 true。
- 由于需要控制子数组长度大于等 2，因此每次计算出的 $sum[j]\%k$ 的值，不能立即放入字典中，而是引入一个中间变量 prev 缓存我们的值，待下一次计算时再加入字典，以保证满足条件的子数组长度至少为 2。
- 对 k=0 的情形，我们需要特判。



```java
    public boolean checkSubarraySum2(int[] nums, int k) {
        Set<Integer> set = new HashSet<>();
        int prev = 0;
        int sum = 0;
        for (int num : nums) {
            sum += num;
            sum = k != 0 ? sum % k : sum;
            if (set.contains(sum)) {
                return true;
            }
            set.add(prev);
            prev = sum;
        }
        return false;
    }
```

时间复杂度：O(n)。
空间复杂度：O(min(n,k))。