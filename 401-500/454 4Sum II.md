
# 454. 4Sum II(M)

[454. 四数相加 II](https://leetcode-cn.com/problems/4sum-ii/)

## 题目描述(中等)

给定四个包含整数的数组列表 `A , B , C , D` ,计算有多少个元组 `(i, j, k, l)` ，使得 `A[i] + B[j] + C[k] + D[l] = 0`。

为了使问题简单化，所有的 A, B, C, D 具有相同的长度 N，且 0 ≤ N ≤ 500 。所有整数的范围在 -2^28 到 2^28 - 1 之间，最终结果不会超过 2^31 - 1 。

例如:
```
输入:
A = [ 1, 2]
B = [-2,-1]
C = [-1, 2]
D = [ 0, 2]

输出:
2

解释:
两个元组如下:
1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0
```

## 思路

- 暴力法
- 分治 哈希表缓存

## 解决方法

### 暴力法

!> 超时

```java
    public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
        int n = A.length;
        int count = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                for (int k = 0; k < n; k++) {
                    for (int l = 0; l < n; l++) {
                        if (A[i] + B[j] + C[k] + D[l] == 0) {
                            count++;
                        }
                    }
                }
            }
        }
        return count;
    }

```

时间复杂度：O(n^4)

**暴力法优化**

遍历 + 二分

!> 超时

```java
    public int fourSumCount_1(int[] A, int[] B, int[] C, int[] D) {
        int n = A.length;
        Arrays.sort(D);
        int count = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                for (int k = 0; k < n; k++) {
                    int left = 0, right = n - 1;
                    while (left <= right) {
                        int l = (left + right) >>> 1;
                        int sum = A[i] + B[j] + C[k] + D[l];
                        if (sum < 0) {
                            left++;
                        } else if (sum > 0) {
                            right--;
                        } else {
                            count++;
                            break;
                        }
                    }
                }
            }
        }
        return count;
    }
```

时间复杂度：O(n^3log(n))

### 分治 哈希表缓存

建立两个哈希映射，一个记录AB数组的组合和，另一个记录CD数组的组合和，和为key，出现的次数为value

遍历哈希表1，若其key的相反数 -key 存在于哈希表2中，次数乘积添加到结果中

```java
    public int fourSumCount1(int[] A, int[] B, int[] C, int[] D) {
        Map<Integer, Integer> countSum1 = new HashMap<>();
        Map<Integer, Integer> countSum2 = new HashMap<>();
        int n = A.length;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                int sum1 = A[i] + B[j];
                if (!countSum1.containsKey(sum1)) {
                    countSum1.put(sum1, 0);
                }
                countSum1.put(sum1, countSum1.get(sum1) + 1);
                int sum2 = C[i] + D[j];
                if (!countSum2.containsKey(sum2)) {
                    countSum2.put(sum2, 0);
                }
                countSum2.put(sum2, countSum2.get(sum2) + 1);
            }
        }
        int count = 0;
        for (int sum1 : countSum1.keySet()) {
            if (countSum2.containsKey(-sum1)) {
                count += countSum1.get(sum1) * countSum2.get(-sum1);
            }
        }
        return count;
    }
```

建立一个哈希映射，一个记录AB数组的组合和，和为key，出现的次数为value
计算CD数组的组合和，得到相反数，若该数存在于key中，即符合要求，将答案加上AB组合和中该数出现的次数(value)


```java
    public int fourSumCount2(int[] A, int[] B, int[] C, int[] D) {
        Map<Integer, Integer> countSum1 = new HashMap<>();
        int n = A.length;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                int sum1 = A[i] + B[j];
                if (!countSum1.containsKey(sum1)) {
                    countSum1.put(sum1, 0);
                }
                countSum1.put(sum1, countSum1.get(sum1) + 1);
            }
        }
        int count = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                int sum2 = C[i] + D[j];
                if (countSum1.containsKey(-sum2)) {
                    count += countSum1.get(-sum2);
                }
            }
        }
        return count;
    }
```