
# 491. Increasing Subsequences(M)

[491. 递增子序列](https://leetcode-cn.com/problems/increasing-subsequences/)

## 题目描述(中等)

给定一个整型数组, 你的任务是找到所有该数组的递增子序列，递增子序列的长度至少是2。

示例:
```
输入: [4, 6, 7, 7]
输出: [[4, 6], [4, 7], [4, 6, 7], [4, 6, 7, 7], [6, 7], [6, 7, 7], [7,7], [4,7,7]]
```

**说明**:
- 给定数组的长度不会超过15。
- 数组中的整数范围是 [-100,100]。
- 给定数组中可能包含重复数字，相等的数字应该被视为递增的一种情况。


## 思路

- 动态规划
- DFS

## 解决方法

### 动态规划 Set去重

记录下以nums[i]为结尾的上升子序列：`Map<Integer, Set<List<Integer>>> map`,利用Set去重上升子序列

```java
    public List<List<Integer>> findSubsequences(int[] nums) {
        Map<Integer, Set<List<Integer>>> map = new HashMap<>();
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            if (!map.containsKey(nums[i])) {
                map.put(nums[i], new HashSet<>());
            }
            boolean hasEqual = false;
            Set<List<Integer>> set = new HashSet<>(map.get(nums[i]));
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i] || (!hasEqual && nums[j] == nums[i])) {
                    set.add(Arrays.asList(nums[j], nums[i]));
                    if (!map.containsKey(nums[j])) {
                        continue;
                    }
                    for (List<Integer> list : map.get(nums[j])) {
                        List<Integer> tmp = new ArrayList<>(list);
                        tmp.add(nums[i]);
                        set.add(tmp);
                    }
                    hasEqual = nums[j] == nums[i];
                }
            }
            map.put(nums[i], set);
        }
        List<List<Integer>> listList = new ArrayList<>();
        for (Set<List<Integer>> set : map.values()) {
            listList.addAll(set);
        }
        return listList;
    }
```

### DFS

list 存储当前上升子序列

```java
    public List<List<Integer>> findSubsequences1(int[] nums) {
        Set<List<Integer>> set = new HashSet<>();
        dfs(nums, 0, new LinkedList<>(), set);
        return new ArrayList<>(set);
    }

    public void dfs(int[] nums, int index, LinkedList<Integer> list, Set<List<Integer>> set) {
        if (list.size() > 1) {
            set.add(new ArrayList<>(list));
        }
        for (int i = index; i < nums.length; i++) {
            if (list.isEmpty() || nums[i] >= list.getLast()) {
                list.addLast(nums[i]);
                dfs(nums, i + 1, list, set);
                list.removeLast();
            }
        }
    }
```

### DFS 比较去重

prev 当前上升子序列的前一项

当前DFS遍历循环中，有重复的值可以跳过，只计算一次

```java
    public List<List<Integer>> findSubsequences2(int[] nums) {
        List<List<Integer>> listList = new ArrayList<>();
        dfs(nums, 0, Integer.MIN_VALUE, new LinkedList<>(), listList);
        return listList;
    }

    public void dfs(int[] nums, int index, int prev, LinkedList<Integer> list, List<List<Integer>> listList) {
        if (list.size() > 1) {
            listList.add(new ArrayList<>(list));
        }
        for (int i = index; i < nums.length; i++) {
            if (nums[i] < prev) {
                continue;
            }
            boolean hasEqual = false;
            for (int j = index; j < i; j++) {
                if (nums[j] == nums[i]) {
                    hasEqual = true;
                    break;
                }
            }
            if (!hasEqual) {
                list.addLast(nums[i]);
                dfs(nums, i + 1, nums[i], list, listList);
                list.removeLast();
            }
        }
    }

```