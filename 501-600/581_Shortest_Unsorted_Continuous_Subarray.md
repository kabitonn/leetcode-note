
# 581. Shortest Unsorted Continuous Subarray(E)
 
[581. 最短无序连续子数组](https://leetcode-cn.com/problems/shortest-unsorted-continuous-subarray/)

## 题目描述(简单)

给定一个整数数组，你需要寻找一个连续的子数组，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。

你找到的子数组应是最短的，请输出它的长度。

示例 1:
```
输入: [2, 6, 4, 8, 10, 9, 15]
输出: 5
解释: 你只需要对 [6, 4, 8, 10, 9] 进行升序排序，那么整个表都会变为升序排序。
```

**说明**:
- 输入的数组长度范围在 [1, 10,000]。
- 输入的数组可能包含重复元素 ，所以升序的意思是<=。


## 思路

找到乱序位置的两端索引

## 解决方法

### 排序后比较两端的乱序位置

```java
    public int findUnsortedSubarray(int[] nums) {
        int n = nums.length;
        int[] array = Arrays.copyOf(nums, n);
        Arrays.sort(array);
        int left = 0, right = n - 1;
        while (left <= right && nums[left] == array[left]) {
            left++;
        }
        while (left <= right && nums[right] == array[right]) {
            right--;
        }
        return right - left + 1;
    }
```

### 双向遍历比较获取乱序位置

- 从左到右循环，记录最大值为 max，若 nums[i] < max, 则表明位置 i 需要调整, 循环结束，记录需要调整的最大位置 i 为 right; 
- 从右到左循环，记录最小值为 min, 若 nums[i] > min, 则表明位置 i 需要调整，循环结束，记录需要调整的最小位置 i 为 left.

```java
    public int findUnsortedSubarray1(int[] nums) {
        int n = nums.length;
        if (n <= 1) {
            return 0;
        }
        int left = n - 1, right = 0;
        int max = nums[0], min = nums[n - 1];
        for (int i = 1; i < n; i++) {
            if (nums[i] < max) {
                right = i;
            } else {
                max = nums[i];
            }
        }
        for (int i = n - 2; i >= 0; i--) {
            if (nums[i] > min) {
                left = i;
            } else {
                min = nums[i];
            }
        }
        return left > right ? 0 : right - left + 1;
    }
```