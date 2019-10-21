# 090. Subsets II(M)
[090. 子集 II](https://leetcode-cn.com/problems/subsets-ii/)


## 题目描述(中等)

Given a collection of integers that might contain duplicates, nums, return all possible subsets (the power set).

Note: The solution set must not contain duplicate subsets.

Example:
```
Input: [1,2,2]
Output:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]

```

## 思路

- 回溯
- 迭代
- 位运算

## 解决方法


### 回溯

```java

    public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> listList = new ArrayList<>();
        Arrays.sort(nums);
        combine(listList, new ArrayList<>(), nums, 0);
        return listList;
    }

    //回溯
    private void combine(List<List<Integer>> listList, List<Integer> list, int[] nums, int index) {
        listList.add(new ArrayList<>(list));
        for (int i = index; i < nums.length; i++) {
            if (i > index && nums[i] == nums[i - 1]) {
                continue;
            }
            list.add(nums[i]);
            combine(listList, list, nums, i + 1);
            list.remove(list.size() - 1);
        }
    }
```

### 迭代



### 位运算



