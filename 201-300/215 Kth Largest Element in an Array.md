# 215. Kth Largest Element in an Array(M)

[](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

## 题目描述(中等)

Find the **kth** largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

Example 1:
```
Input: [3,2,1,5,6,4] and k = 2
Output: 5
```
Example 2:
```
Input: [3,2,3,1,2,4,5,5,6] and k = 4
Output: 4
```
**Note**:
You may assume k is always valid, 1 ≤ k ≤ array's length.

## 思路

- 完全排序
- 快排
- 堆排


## 解决方法

### 完全排序

```java
        public int findKthLargest0(int[] nums, int k) {
        Arrays.sort(nums);
        return nums[nums.length - k];
    }

```

### 快排

快排中每趟quickpass(partition)返回的下标即为所有数组中该元素最后应该在的位置，判断该下标和目标确定搜索区间

普通快排，默认选择区间left为pivot元素
```java
    public int findKthLargest(int[] nums, int k) {
        int target = nums.length - k;
        int left = 0, right = nums.length - 1;
        while (left < right) {
            int pivot = quickPass(nums, left, right);
            if (pivot == target) {
                break;
            } else if (pivot > target) {
                right = pivot - 1;
            } else if (pivot < target) {
                left = pivot + 1;
            }
        }
        return nums[target];
    }

    public int quickPass(int[] nums, int left, int right) {
        int temp = nums[left];
        while (left < right) {
            while (left < right && nums[right] >= temp) {
                right--;
            }
            nums[left] = nums[right];
            while (left < right && nums[left] <= temp) {
                left++;
            }
            nums[right] = nums[left];
        }
        nums[left] = temp;
        return left;
    }

```

修改快排pivot 的选取，选取中位数
```java
    public int findKthLargest1(int[] nums, int k) {
        return quickSelect(nums, 0, nums.length - 1, nums.length - k);
    }

    public int quickSelect(int[] nums, int left, int right, int k) {
        if (left == right) {
            return nums[left];
        }
        int mid = left + (right - left) / 2;
        swap(nums, left, mid);
        int temp = nums[left];
        int start = left, end = right;
        while (left < right) {
            while (left < right && nums[right] >= temp) {
                right--;
            }
            nums[left] = nums[right];
            while (left < right && nums[left] <= temp) {
                left++;
            }
            nums[right] = nums[left];
        }
        nums[left] = temp;
        if (left < k) {
            return quickSelect(nums, left + 1, end, k);
        } else if (left > k) {
            return quickSelect(nums, start, left - 1, k);
        } else {
            return nums[left];
        }
    }

    public void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }

```