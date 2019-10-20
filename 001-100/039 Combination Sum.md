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

### 动态规划 set去重

```java
public List<List<Integer>> combinationSum1(int[] candidates, int target) {

        List<List<Integer>> result = new ArrayList<>();
        Map<Integer, Set<List<Integer>>> map = new HashMap<>();
        //对candidates数组进行排序
        Arrays.sort(candidates);
        int len = candidates.length;
        for (int i = 1; i <= target; i++) {
            //初始化map
            map.put(i, new HashSet<>());
            //对candidates数组进行循环
            for (int j = 0; j < len && candidates[j] <= target; j++) {
                if (i == candidates[j]) {
                    //相等即为相减为0的情况，直接加入set集合即可
                    List<Integer> temp = new ArrayList<>();
                    temp.add(i);
                    map.get(i).add(temp);
                } else if (i > candidates[j]) {
                    //i-candidates[j]是map的key
                    int key = i - candidates[j];
                    //使用迭代器对对应key的set集合进行遍历
                    //如果candidates数组不包含这个key值，对应的set集合会为空，故这里不需要做单独判断
                    for (Iterator iterator = map.get(key).iterator(); iterator.hasNext(); ) {
                        List list = (List) iterator.next();
                        //set集合里面的每一个list都要加入candidates[j]，然后放入到以i为key的集合中
                        List tempList = new ArrayList<>();
                        tempList.addAll(list);
                        tempList.add(candidates[j]);
                        //排序是为了通过set集合去重
                        Collections.sort(tempList);
                        map.get(i).add(tempList);
                    }
                }
            }
        }
        result.addAll(map.get(target));

        return result;
    }

```


### 动态规划 外层遍历nums

可以把两个 for 循环的遍历颠倒一下，外层遍历 nums，内层遍历 dp

```java
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>>[] dp = new ArrayList[target + 1];
        for (int i = 0; i <= target; i++) {
            dp[i] = new ArrayList<>();
        }
        Arrays.sort(candidates);
        for (int n : candidates) {
            for (int sum = n; sum <= target; sum++) {
                if (sum == n) {
                    List<Integer> temp = new ArrayList<>();
                    temp.add(n);
                    dp[n].add(temp);
                }
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