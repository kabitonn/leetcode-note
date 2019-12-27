# 039. Combination Sum(M)
[039. Combination Sum](https://leetcode-cn.com/problems/combination-sum/)

## 题目描述

Given a set of candidate numbers (candidates) (without duplicates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.

The same repeated number may be chosen from candidates unlimited number of times.

**Note**:

> - All numbers (including target) will be positive integers.
> - The solution set must not contain duplicate combinations.

Example 1:
```
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
```
Example 2:
```
Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

## 思路

回溯

## 解决方法

### 递归回溯

```java
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> list = new ArrayList<>();
        combine(list, new ArrayList<>(), candidates, target, 0);
        return list;
    }

    public void combine(List<List<Integer>> list, List<Integer> line, int[] nums, int remain, int start) {
        if (remain < 0) {
            return;
        }
        if (remain == 0) {
            list.add(new ArrayList<>(line));
            return;
        }
        for (int i = start; i < nums.length; i++) {
            line.add(nums[i]);
            combine(list, line, nums, remain - nums[i], i);
            line.remove(line.size() - 1);
        }
    }

```
时间复杂度：

空间复杂度：

### 回溯 排序剪枝

剪枝 因为已经将candidates排序， remain < candidates[i]之前，进行截断。


```java
        public List<List<Integer>> combinationSum0(int[] candidates, int target) {
        List<List<Integer>> list = new ArrayList<>();
        Arrays.sort(candidates);
        combine0(list, new LinkedList<>(), candidates, target, 0);
        return list;
    }

    public void combine0(List<List<Integer>> list, LinkedList<Integer> line, int[] nums, int remain, int start) {
        if (remain == 0) {
            list.add(new ArrayList<>(line));
            return;
        }
        for (int i = start; i < nums.length && remain >= nums[i]; i++) {
            line.add(nums[i]);
            combine0(list, line, nums, remain - nums[i], i);
            line.removeLast();
        }
    }

```

### 动态规划 set去重

可能会出现[2,3]和[3,2]的重复情况

```java
    public List<List<Integer>> combinationSum1(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<>();
        Map<Integer, Set<List<Integer>>> map = new HashMap<>();
        map.put(0, new HashSet<>());
        map.get(0).add(new ArrayList<>());
        //对candidates数组进行排序
        Arrays.sort(candidates);
        int len = candidates.length;
        for (int i = 1; i <= target; i++) {
            //初始化map
            map.put(i, new HashSet<>());
            //对candidates数组进行循环
            for (int candidate : candidates) {
                if (candidate > i) {
                    continue;
                }
                //i-candidate是map的key
                int key = i - candidate;
                //使用迭代器对对应key的set集合进行遍历
                //如果candidates数组不包含这个key值，对应的set集合会为空，故这里不需要做单独判断
                for (List list : map.get(key)) {
                    //set集合里面的每一个list都要加入candidates[j]，然后放入到以i为key的集合中
                    List tempList = new ArrayList<>(list);
                    tempList.add(candidate);
                    //排序是为了通过set集合去重
                    Collections.sort(tempList);
                    map.get(i).add(tempList);
                }
            }
        }
        result.addAll(map.get(target));
        return result;
    }
```


### 动态规划 外层遍历nums

dp[i][j]:前n项组合成j的组合List

排序后dp[i] 只和dp[i-1]有关

可以把两个 for 循环的遍历颠倒一下，外层遍历 nums，内层遍历 dp，这样可以避免重复List的情况

```java
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>>[] dp = new ArrayList[target + 1];
        for (int i = 0; i <= target; i++) {
            dp[i] = new ArrayList<>();
        }
        dp[0].add(new ArrayList<>());
        Arrays.sort(candidates);
        for (int n : candidates) {
            for (int sum = n; sum <= target; sum++) {
                int key = sum - n;
                for (List list : dp[key]) {
                    List tempList = new ArrayList(list);
                    tempList.add(n);
                    dp[sum].add(tempList);
                }
            }
        }
        return dp[target];
    }
```