
# 462. Minimum Moves to Equal Array Elements II(M)

[462. 最少移动次数使数组元素相等 II](https://leetcode-cn.com/problems/minimum-moves-to-equal-array-elements-ii/)

## 题目描述(中等)

给定一个非空整数数组，找到使所有数组元素相等所需的最小移动数，其中每次移动可将选定的一个元素加1或减1。 您可以假设数组的长度最多为10000。

例如:
```
输入:
[1,2,3]

输出:
2

说明：
只有两个动作是必要的（记得每一步仅可使其中一个元素加1或减1）： 

[1,2,3]  =>  [2,2,3]  =>  [2,2,2]

```

## 思路

- 寻找中间的数
- 两端数字向中间变化

## 解决方法

### 排序 中位数

假设最终数组 a 中的每个数均为 x，那么需要移动的次数即为 |a[0] - x| + |a[1] - x| + ... + |a[n-1] - x|。如果我们把数组 a 中的每个数看成水平轴上的一个点，那么根据上面的移动次数公式，我们需要找到在水平轴上找到一个点 x，使得这 N 个点到 x 的距离之和最小。

这是一个经典的数学问题，当 x 为这个 N 个数的中位数时，可以使得距离最小。具体地，若 N 为奇数，则 x 必须为这 N 个数中的唯一中位数；若 N 为偶数，中间的两个数为 p 和 q，中位数为 (p + q) / 2，此时 x 只要是区间 [p, q] 中的任意一个数即可。


排序得到中位数，所有数字变化为中位数

```java
    public int minMoves2(int[] nums) {
        if (nums.length == 0) {
            return 0;
        }
        Arrays.sort(nums);
        int target = nums[nums.length / 2];
        int sum = 0;
        for (int n : nums) {
            sum += Math.abs(target - n);
        }
        return sum;
    }

```

时间复杂度：O(NlogN)，其中 N 是数组的长度。

空间复杂度：O(logN)，为排序需要使用的空间。

### 快速选择 中位数

```java
    public int minMoves2_4(int[] nums) {
        int len = nums.length;
        int k = len / 2;
        int target = quickSelect(nums, 0, len - 1, k);
        int sum = 0;
        for (int n : nums) {
            sum += Math.abs(target - n);
        }
        return sum;
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
```

时间复杂度：平均为 O(N)，但在最坏情况下会达到 O(N^2)，这是快速排序本身的性质导致的。

空间复杂度：O(logN)，为快速选择需要使用的空间。


### 小根堆 排序得到中位数

```java
    public int minMoves2_2(int[] nums) {
        int len = nums.length;
        int size = (len + 1) / 2;
        buildMinHeap(nums);
        for (int unsorted = len; unsorted > size; ) {
            swap(nums, 0, --unsorted);
            adjustMin(nums, 0, unsorted);
        }
        int target = nums[0];
        int sum = 0;
        for (int n : nums) {
            sum += Math.abs(target - n);
        }
        return sum;
    }

    public void buildMinHeap(int[] arr) {
        int len = arr.length;
        for (int i = len / 2; i >= 0; i--) {
            adjustMin(arr, i, len);
        }
    }

    public void adjustMin(int[] arr, int node, int size) {
        int left = (node << 1) + 1;
        int right = left + 1;
        while (left < size) {
            int small = right < size && arr[right] < arr[left] ? right : left;
            if (arr[node] <= arr[small]) {
                break;
            }
            swap(arr, small, node);
            node = small;
            left = (node << 1) + 1;
            right = left + 1;
        }
    }

    public void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }

```

时间复杂度：O(NlogN)，其中 N 是数组的长度。

空间复杂度：O(1)

### 排序 两端向中间变化

设 a <= x <= b，将 a 和 b 都变化成 x 为最终目的，则需要步数为 x-a+b-x = b-a，即两个数最后相等的话步数一定是他们的差，x 在 a 和 b 间任意取；
所以最后剩的其实就是中位数；

```java
    public int minMoves2_1(int[] nums) {
        Arrays.sort(nums);
        int left = 0;
        int right = nums.length - 1;
        int sum = 0;
        while (left < right) {
            sum += nums[right--] - nums[left++];
        }
        return sum;
    }
```

时间复杂度：O(NlogN)，其中 N 是数组的长度。

空间复杂度：O(logN)，为排序需要使用的空间。