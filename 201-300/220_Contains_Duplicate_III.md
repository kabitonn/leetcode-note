# 220. Contains Duplicate III(M)

[](https://leetcode-cn.com/problems/contains-duplicate-iii/)

## 题目描述(中等)

Given an array of integers, find out whether there are two distinct indices i and j in the array such that the **absolute** difference between **nums[i]** and **nums[j]** is at most t and the **absolute **difference between i and j is at most k.

Example 1:
```
Input: nums = [1,2,3,1], k = 3, t = 0
Output: true
```
Example 2:
```
Input: nums = [1,0,1,1], k = 1, t = 2
Output: true
```
Example 3:
```
Input: nums = [1,5,9,1,5,9], k = 2, t = 3
Output: false
```

## 思路

- 遍历
- 滑动窗口


## 解决方法

### 遍历


```java
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        int len = nums.length;
        for (int i = 0; i < len - 1; i++) {
            for (int j = i + 1; j <= i + k && j < len; j++) {
                long diff = Math.abs((long) nums[j] - (long) nums[i]);
                if (diff <= t) {
                    return true;
                }
            }
        }
        return false;
    }

```
时间复杂度：$O(n * \min(k,n))$每次搜索都要花费 $O(\min(k, n))$ 的时间，哪怕k比n大，一次搜索中也只需比较 n 次。

空间复杂度：O(1)



### 滑动窗口

```java
    public boolean containsNearbyAlmostDuplicate1(int[] nums, int k, int t) {
        if (nums == null || nums.length < 2 || k <= 0) {
            return false;
        }
        int i = 0, j = i + 1;
        int len = nums.length;
        while (i < len - 1) {
            long diff = Math.abs((long) nums[j] - (long) nums[i]);
            if (diff <= t && j - i <= k) {
                return true;
            }
            if (j - i < k && j < len - 1) {
                j++;
            } else {
                i++;
                j = i + 1;
            }
        }
        return false;
    }
```

上述代码已退化成遍历，因为i取下一位时，j同时也回退到i的下一位

修改j的回退逻辑，当t==0时，说明只有重复数字才符合要求，i到j之间遍历过的的数没有和i重复的，但是i和j之间就有重复的；

下述代码通过测试，但逻辑有bug

```
[1,2,2,4,1] k=3,t=1

i=0时，j可以走到3，就会跳过1和2两个重复的值
```

```java
    public boolean containsNearbyAlmostDuplicate2(int[] nums, int k, int t) {
        if (nums == null || nums.length < 2 || k <= 0) {
            return false;
        }
        int i = 0, j = i + 1;
        int len = nums.length;
        while (i < len - 1) {
            long diff = Math.abs((long) nums[j] - (long) nums[i]);
            if (diff <= t && j - i <= k) {
                return true;
            }
            if (j - i < k && j < len - 1) {
                j++;
            } else {
                i++;
                if (t != 0) {
                    j = i + 1;
                }
            }
        }
        return false;
    }
```