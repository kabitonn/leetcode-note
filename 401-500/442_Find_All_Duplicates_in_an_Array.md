
# 442. Find All Duplicates in an Array(M)

[442. 数组中重复的数据](https://leetcode-cn.com/problems/find-all-duplicates-in-an-array/)

## 题目描述(中等)

给定一个整数数组 a，其中`1 ≤ a[i] ≤ n` （n为数组长度）, 其中有些元素出现两次而其他元素出现一次。

找到所有出现两次的元素。

你可以不用到任何额外空间并在O(n)时间复杂度内解决这个问题吗？

示例：
```
输入:
[4,3,2,7,8,2,3,1]

输出:
[2,3]
```

## 思路

- 计数
- 抽屉原理

## 解决方法

### 桶排序 计数

```java
    public List<Integer> findDuplicates(int[] nums) {
        int n = nums.length;
        int[] arr = new int[n];
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            arr[nums[i] - 1]++;
            if (arr[nums[i] - 1] >= 2) {
                list.add(nums[i]);
            }
        }
        return list;
    }
```

时间复杂度 O(n)

空间复杂度 O(n)

### 原地操作 增加数值

利用索引和值的关系 抽屉原理

`1 ≤ nums[i] ≤ n`，对于出现的数字n在其应对应的索引n-1的位置上添加数值n，最后若某个索引上的值若大于2n表示，该索引i对应的数字i+1出现了两次

```java
    public List<Integer> findDuplicates2(int[] nums) {
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            int index = (nums[i] - 1) % n;
            nums[index] += n;
        }
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            if (nums[i] > n * 2) {
                list.add(i + 1);
            }
        }
        return list;
    }
```

时间复杂度 O(n)

空间复杂度 O(1)

### 原地操作 交换

利用抽屉原理，数字n应放在n-1的索引上，除非n-1索引上已经是n了，

最后在索引i上出现的不是i+1,而是n，说明n出现了两次

```java
    public List<Integer> findDuplicates1(int[] nums) {
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            while (nums[i] != nums[nums[i] - 1]) {
                swap(nums, i, nums[i] - 1);
            }
        }
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            if (i + 1 != nums[i]) {
                list.add(nums[i]);
            }
        }
        return list;
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

时间复杂度 O(n)

空间复杂度 O(1)