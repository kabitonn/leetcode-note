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
递归交换


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

### 回溯 排序去重

判断该数之前有没有和它相等的，并且两数没有同时被访问(即两数在该循环中都已经不再变化)，若有则跳过该数


```java
    public List<List<Integer>> permuteUnique1(int[] nums) {
        List<List<Integer>> listList = new ArrayList<>();
        boolean[] visited = new boolean[nums.length];
        Arrays.sort(nums);
        backtrack1(nums, visited, listList, new ArrayList<>());
        return listList;
    }

    private void backtrack1(int[] nums, boolean[] visited, List<List<Integer>> listList, List<Integer> list) {
        if (list.size() == nums.length) {
            listList.add(new ArrayList<>(list));
        }
        for (int i = 0; i < visited.length; i++) {
            if (i > 0 && nums[i] == nums[i - 1] && !visited[i - 1]) {
                continue;
            }
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

### 递归 交换

在每一个位置，让每个数字轮流交换过去一下。这里的话，其实只要把当前位置已经有哪些数字来过保存起来，如果有重复的话，我们不让他交换，直接换下一个数字就可以了

也可以判断该数之前有没有和它相等的，若有则跳过该数

```java
    public List<List<Integer>> permuteUnique3(int[] nums) {
        List<List<Integer>> listList = new ArrayList<>();
        Arrays.sort(nums);
        permute(nums, 0, listList);
        return listList;
    }

    private void permute(int[] nums, int start, List<List<Integer>> listList) {
        if (start == nums.length) {
            //List list = Arrays.stream(nums).boxed().collect(Collectors.toList());
            List<Integer> list = new ArrayList<>();
            for (int n : nums) {
                list.add(n);
            }
            listList.add(list);
        }
        Set<Integer> set = new HashSet<>();
        for (int i = start; i < nums.length; i++) {
            /*int j = i - 1;
            while (j >= start && nums[j] != nums[i]) {
                --j;
            }
            if (j != start - 1) {
                continue;
            }*/
            if(set.contains(nums[i])){
                continue;
            }
            set.add(nums[i]);
            swap(nums, i, start);
            permute(nums, start + 1, listList);
            swap(nums, i, start);
        }
    }

    public void swap(int[] nums, int i, int j) {
        if (i == j) {
            return;
        }
        nums[i] = nums[i] ^ nums[j];
        nums[j] = nums[i] ^ nums[j];
        nums[i] = nums[i] ^ nums[j];
    }
```

### 迭代 动态规划 set去重

```java
    public List<List<Integer>> permuteUnique4(int[] nums) {
        Set<List<Integer>> setList = new HashSet<>();

        setList.add(new ArrayList<>());
        //在上边的基础上只加上最外层的 for 循环就够了，代表每次新添加的数字
        for (int i = 0; i < nums.length; i++) {
            Set<List<Integer>> set = new HashSet<>();
            for (List list : setList) {
                for (int k = 0; k <= i; k++) {
                    List<Integer> temp = new ArrayList<>(list);
                    temp.add(k, nums[i]);
                    set.add(temp);
                }
            }
            setList = set;
        }
        return new ArrayList<>(setList);
    }
```

