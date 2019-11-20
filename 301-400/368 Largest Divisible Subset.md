# 368. Largest Divisible Subset(M)

[368. 最大整除子集](https://leetcode-cn.com/problems/largest-divisible-subset/)

## 题目描述(中等)

给出一个由无重复的正整数组成的集合，找出其中最大的整除子集，子集中任意一对 (Si，Sj) 都要满足：`Si % Sj = 0` 或 `Sj % Si = 0`。

如果有多个目标子集，返回其中任何一个均可。

 

示例 1:
```
输入: [1,2,3]
输出: [1,2] (当然, [1,3] 也正确)
```
示例 2:
```
输入: [1,2,4,8]
输出: [1,2,4,8]
```

## 思路

动态规划

## 解决方法

### 动态规划

第i个元素如果想要加入前面某个元素的最大整除子集，那么它需要能够整除前面这个元素（这个元素的整除子集都可以整除它自身，如果第i个元素能够被这个元素整除，自然也可以被它的整除子集全部整除），此时，第i个元素的最大整除子集就是前面所有能整除第i个元素的元素的最大整除子集+1
在这种情况下，每个元素仅需要记录自己的最大整除子集的前置元素即可。


dp[i] : 以i为结尾的最大整数子集个数

dp[i] = max(dp[j]+1 if nums[i]%nums[j]==0 && j < i)

end：最大整数子集的最后一个索引

last: 整数子集中前一个索引

```java
    public List<Integer> largestDivisibleSubset(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        int[] dp = new int[n];
        int max = 0;
        int[] last = new int[n];
        int end = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] % nums[j] == 0 && dp[i] <= dp[j]) {
                    dp[i] = dp[j];
                    last[i] = j;
                }
            }
            dp[i]++;
            if (dp[i] > max) {
                max = dp[i];
                end = i;
            }
        }
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < max; i++) {
            list.add(0, nums[end]);
            end = last[end];
        }
        return list;
    }

```
