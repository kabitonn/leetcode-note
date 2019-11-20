# 324. Wiggle Sort II(M)

[324. 摆动排序 II](https://leetcode-cn.com/problems/wiggle-sort-ii/)

## 题目描述(中等)

给定一个无序的数组 nums，将它重新排列成 `nums[0] < nums[1] > nums[2] < nums[3]...` 的顺序。

示例 1:
```
输入: nums = [1, 5, 1, 1, 6, 4]
输出: 一个可能的答案是 [1, 4, 1, 5, 1, 6]
```
示例 2:
```
输入: nums = [1, 3, 2, 2, 3, 1]
输出: 一个可能的答案是 [2, 3, 1, 3, 1, 2]
```
**说明**:
你可以假设所有输入都会得到有效的结果。

**进阶**:
你能用 O(n) 时间复杂度和 / 或原地 O(1) 额外空间来实现吗？


## 思路

- 排序 穿插

## 解决方法

### 排序 穿插

排序后分为两段数组，较小数组可以多一个，因为首个为小数，较大数组((len-1)/2)和较小数组((len+1)/2)，再分别逆序穿插

```java
    public void wiggleSort(int[] nums) {
        int len = nums.length;
        int[] copy = Arrays.copyOf(nums, len);
        Arrays.sort(copy);
        int mid = (len + 1) / 2;
        for (int i = 0; i < mid; i++) {
            nums[i * 2] = copy[mid - 1 - i];
        }
        for (int i = 0; i < len / 2; i++) {
            nums[i * 2 + 1] = copy[len - 1 - i];
        }
    }
```

```java
    public void wiggleSort0(int[] nums) {
        int len = nums.length;
        int[] copy = Arrays.copyOf(nums, len);
        Arrays.sort(copy);
        int mid = (len + 1) / 2 - 1, end = len - 1;
        for (int i = 0; i < nums.length; i++) {
            nums[i] = i % 2 == 0 ? copy[mid--] : copy[end--];
        }
    }
```


对有序数组依此将较大的数字插入序列中，

```java
    public void wiggleSort1(int[] nums) {
        int len = nums.length;
        int[] copy = Arrays.copyOf(nums, len);
        Arrays.sort(copy);
        int j = len - 1;
        for (int i = 0; i * 2 + 1 < len; i++) {
            nums[i * 2 + 1] = copy[j--];
        }
        for (int i = 0; i * 2 < len; i++) {
            nums[i * 2] = copy[j--];
        }
    }
```
