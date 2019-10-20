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

### 迭代(动态规划)

只需要在之前的情况里，数字的空隙插入新的数字就够了

```java
    public List<List<Integer>> permute4(int[] nums) {
        List<List<Integer>> listList = new ArrayList<>();
        listList.add(new ArrayList<>());
        //在上边的基础上只加上最外层的 for 循环就够了，代表每次新添加的数字
        for (int i = 0; i < nums.length; i++) {
            int currentSize = listList.size();
            for (int j = 0; j < currentSize; j++) {
                for (int k = 0; k <= i; k++) {
                    List<Integer> temp = new ArrayList<>(listList.get(j));
                    temp.add(k, nums[i]);
                    listList.add(temp);
                }
            }
            for (int j = 0; j < currentSize; j++) {
                listList.remove(0);
            }
        }
        return listList;
    }
```

### 递归(动态规划)

在之前存在数字的已有情况下，数字空隙插入新的数字即可

```java
    public List<List<Integer>> permute5(int[] nums) {
        return permute5(nums, nums.length);
    }

    public List<List<Integer>> permute5(int[] nums, int len) {
        if (len == 1) {
            List<List<Integer>> listList = new ArrayList<>();
            List<Integer> list = new ArrayList<>();
            list.add(nums[0]);
            listList.add(list);
            return listList;
        }
        List<List<Integer>> preListList = permute5(nums, len - 1);
        List<List<Integer>> listList = new ArrayList<>();
        for (List<Integer> list : preListList) {
            for (int i = 0; i < len; i++) {
                List<Integer> tempList = new ArrayList<>(list);
                tempList.add(i, nums[len - 1]);
                listList.add(tempList);
            }
        }
        return listList;
    }
```
时间复杂度，如果只分析代码的话挺复杂的。如果从最后的结果来说，应该是 n! 个结果，所以时间复杂度应该是 O(n!)。

空间复杂度：O(1)。


