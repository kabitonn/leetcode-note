# 046. Permutations(M)
[046. Permutations](https://leetcode-cn.com/problems/permutations/)

## 题目描述(中等)

Given a collection of distinct integers, return all possible permutations.

Example:
```
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

## 思路

回溯
迭代

## 解决方法



### 回溯

visited记录是否使用过

```java
    public List<List<Integer>> permute1(int[] nums) {
        List<List<Integer>> listList = new ArrayList<>();
        boolean[] visited = new boolean[nums.length];
        backtrack1(nums, visited, listList, new ArrayList<>());
        return listList;
    }

    private void backtrack1(int[] nums, boolean[] visited, List<List<Integer>> listList, List<Integer> list) {
        if (list.size() == nums.length) {
            listList.add(new ArrayList<>(list));

        }
        for (int i = 0; i < visited.length; i++) {
            if (!visited[i]) {
                list.add(nums[i]);
                visited[i] = true;
                backtrack1(nums, visited, listList, list);
                list.remove(list.size() - 1);
                visited[i] = false;
            }
        }
    }

```

```java
    public List<List<Integer>> permute2(int[] nums) {
        List<List<Integer>> listList = new ArrayList<>();
        long visited = 0;
        backtrack2(nums, visited, listList, new ArrayList<>());
        return listList;
    }

    private void backtrack2(int[] nums, long visited, List<List<Integer>> listList, List<Integer> list) {
        if (list.size() == nums.length) {
            listList.add(new ArrayList<>(list));
        }
        for (int i = 0; i < nums.length; i++) {
            if ((1 << i & visited) == 0) {
                list.add(nums[i]);
                visited ^= 1 << i;
                backtrack2(nums, visited, listList, list);
                list.remove(list.size() - 1);
                visited ^= 1 << i;
            }
        }
    }
```

### 交换

