# 047. Permutations II(M)


## 题目描述(中等)

Given a collection of numbers that might contain duplicates, return all possible unique permutations.

Example:
```
Input: [1,1,2]
Output:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```

## 思路

回溯


## 解决方法


### 回溯 set去重

```java
    public List<List<Integer>> permuteUnique(int[] nums) {
        Set<List<Integer>> listSet = new HashSet<>();
        List<List<Integer>> listList = new ArrayList<>();
        boolean[] visited = new boolean[nums.length];
        backtrack(nums, visited, listSet, new ArrayList<>());
        for (List l : listSet) {
            listList.add(l);
        }
        return listList;
    }

    private void backtrack(int[] nums, boolean[] visited, Set<List<Integer>> listSet, List<Integer> list) {
        if (list.size() == nums.length) {
            listSet.add(new ArrayList<>(list));
        }
        for (int i = 0; i < visited.length; i++) {
            if (!visited[i]) {
                list.add(nums[i]);
                visited[i] = true;
                backtrack(nums, visited, listSet, list);
                list.remove(list.size() - 1);
                visited[i] = false;
            }
        }
    }
```