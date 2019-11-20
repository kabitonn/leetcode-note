
# 448. Find All Numbers Disappeared in an Array(E)

[448. 找到所有数组中消失的数字](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/)

## 题目描述(中等)

给定一个范围在  `1 ≤ a[i] ≤ n` ( n = 数组大小 ) 的 整型数组，数组中的元素一些出现了两次，另一些只出现一次。

找到所有在 `[1, n]` 范围之间没有出现在数组中的数字。

您能在不使用额外空间且时间复杂度为O(n)的情况下完成这个任务吗? 你可以假定返回的数组不算在额外空间内。

示例:
```
输入:
[4,3,2,7,8,2,3,1]

输出:
[5,6]
```


## 思路

- 计数
- 抽屉原理

## 解决方法

### 桶排序 计数

```java
    public List<Integer> findDisappearedNumbers1(int[] nums) {
        int[] indexs = new int[nums.length];
        for (int n : nums) {
            indexs[n - 1]++;
        }
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < indexs.length; i++) {
            if (indexs[i] == 0) {
                list.add(i + 1);
            }
        }
        return list;
    }
```

时间复杂度 O(n)

空间复杂度 O(n)

### 原地操作 增加数值

利用索引和值的关系 抽屉原理

`1 ≤ nums[i] ≤ n`，对于出现的数字n在其应对应的索引n-1的位置上添加数值n，最后若某个索引i上的值若大于2n表示该索引i对应的数字i+1出现了两次，若某个索引j上的值小于n表示该索引j对应的数字j+1为缺少值

```java
    public List<Integer> findDisappearedNumbers2(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            int n = (nums[i] - 1) % nums.length;
            nums[n] += nums.length;
        }
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] < nums.length) {
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

最后在索引i上出现的不是i+1,而是n，说明n出现了两次，而i+1是缺少的值

```java
    public List<Integer> findDisappearedNumbers(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            while (nums[nums[i] - 1] != nums[i]) {
                swap(nums, i, nums[i] - 1);
            }
        }
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != i + 1) {
                list.add(i + 1);
            }
        }
        return list;
    }

    public void swap(int[] nums, int i, int j) {
        nums[i] = nums[i] ^ nums[j];
        nums[j] = nums[i] ^ nums[j];
        nums[i] = nums[i] ^ nums[j];
    }
```

时间复杂度 O(n)

空间复杂度 O(1)