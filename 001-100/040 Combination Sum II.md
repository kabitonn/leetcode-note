# 040. Combination Sum II(M)


## 题目描述(中等)

Given a collection of candidate numbers (candidates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.

Each number in candidates may only be used once in the combination.

**Note**:

- All numbers (including target) will be positive integers.
- The solution set must not contain duplicate combinations.

Example 1:
```
Input: candidates = [10,1,2,7,6,1,5], target = 8,
A solution set is:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```
Example 2:
```
Input: candidates = [2,5,2,1,2], target = 5,
A solution set is:
[
  [1,2,2],
  [5]
]
```

## 思路

回溯

## 解决方法

### 递归回溯

跳过重复的数字，有序数组中，若和之前相等，且不是同时存在，前一个必然已经添加入解集中

```java
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> lists = new ArrayList<>();
        Arrays.sort(candidates);
        combine(lists, new ArrayList<>(), candidates, target, 0);
        return lists;
    }

    private void combine(List<List<Integer>> list, List<Integer> line, int[] nums, int remain, int start) {
        if (remain < 0) {
            return;
        }
        if (remain == 0) {
            list.add(new ArrayList<>(line));
            return;
        }
        for (int i = start; i < nums.length; i++) {
            if (i > start && nums[i] == nums[i - 1]) {
                continue;
            }
            line.add(nums[i]);
            combine(list, line, nums, remain - nums[i], i + 1);
            line.remove(line.size() - 1);
        }
    }
```

