# 046. Permutations\(M\)

[046. Permutations](https://leetcode-cn.com/problems/permutations/)

## 题目描述\(中等\)

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

![](/assets/001-100/046-s-1-1.png)
每调用一层就进入一个 for 循环，相当于列出了所有解，然后挑选了我们需要的。其实本质上就是深度优先遍历 DFS。


visited记录是否使用过(数组或二进制位来实现)

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

### 递归 交换

只需要用一个 for 循环，把每一个数字都放到 start 一次

```java
    public List<List<Integer>> permute3(int[] nums) {
        List<List<Integer>> listList = new ArrayList<>();
        permute(nums, 0, listList);
        return listList;
    }

    /**
     * 递归交换
     */
    private void permute(int[] nums, int start, List<List<Integer>> listList) {
        if (start == nums.length) {
            //List list = Arrays.stream(nums).boxed().collect(Collectors.toList());
            List<Integer> list = new ArrayList<>();
            for(int n:nums){
                list.add(n);
            }
            listList.add(list);
        }
        for (int i = start; i < nums.length; i++) {
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



