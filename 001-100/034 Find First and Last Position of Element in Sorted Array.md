## [34. Find First and Last Position of Element in Sorted Array](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

## 1. 题目描述\(中等\)

Given an array of integers nums sorted in ascending order, find the starting and ending position of a given target value.

Your algorithm's runtime complexity must be in the order of $$O(log n)$$.

If the target is not found in the array, return \[-1, -1\].

Example 1:

```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

Example 2:

```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

## 2. 思路

二分查找

## 3. 解决方法

### 3.1 一般二分查找+前后遍历

一般二分查找找到相等值索引，前后遍历找到最小最大索引

```java
    public int[] searchRange(int[] nums, int target) {
        int position = binarySearch(nums, target);
        if(position!=-1) {
            int start = position;
            int end = position;
            while(start>0 && nums[start-1]==target) {start--;}
            while(end<nums.length-1 && nums[end+1]==target) {end++;}
            return new int[] {start,end};
        }
        return new int[]{-1,-1};
    }    
    public int binarySearch(int[] nums, int target) {
        int low = 0;
        int high = nums.length-1;
        while(low<=high) {
            int mid = (low+high)/2;
            if(nums[mid]==target) {    return mid;}
            else if (nums[mid]>target) {    high = mid - 1;}
            else if (nums[mid]<target) {    low = mid + 1;}
        }
        return -1;
    }
```

### 3.2 改进二分查找

修改二分查找，找到和目标值相等的索引最小的值，向后遍历找到索引最大值

```java
    public int[] searchRange(int[] nums, int target) {
        int left = binarySearchMin(nums, target);
        if(left!=-1) {
            int start = left;
            int end = start;
            while(end<nums.length-1 && nums[end+1]==target) {end++;}
            return new int[] {start,end};
        }
        return new int[]{-1,-1};
    }
    public int binarySearchMin(int[] nums, int target) {
        int low = 0;
        int high = nums.length;
        while(low<high) {
            int mid = (low+high)/2;
            if (nums[mid]<target) {    low = mid + 1;}
            else if(nums[mid]>=target){    high = mid;}
        }
        low = (low<nums.length && nums[low]==target)?low:-1;
        return low;
    }
```

修改二分查找，找到和目标值相等的索引最大的值，向前遍历找到索引最大值

```java
    public int[] searchRange(int[] nums, int target) {
        int right = binarySearchMin(nums, target);
        if(right!=-1) {
            int start = right;
            int end = start;
            while(start>0 && nums[start-1]==target) {start--;}
            return new int[] {start,end};
        }
        return new int[]{-1,-1};
    }
    public int binarySearchMax(int[] nums, int target) {
        int low = 0;
        int high = nums.length;
        while(low<high) {
            int mid = (low+high)/2;
            if (nums[mid]<=target) {    low = mid + 1;}
            else if(nums[mid]>target){    high = mid;}
        }
        low = (low>0 && nums[low-1]==target)?low-1:-1;
        return low;
    }
```

### 3.3 二分查找改

```java
    public int[] searchRange(int[] nums, int target) {
        int left = binarySearchMin(nums, target);
        int right = binarySearchMax(nums, target);

        return new int[]{left,right};
    }
        public int binarySearchMin(int[] nums, int target) {
        int low = 0;
        int high = nums.length;    //注意
        while(low<high) {    //注意
            int mid = (low+high)/2;
            if (nums[mid]<target) {    low = mid + 1;}
            else if(nums[mid]>=target){    high = mid;}//注意
        }
        low = (low<nums.length && nums[low]==target)?low:-1;
        return low;
    }
    public int binarySearchMax(int[] nums, int target) {
        int low = 0;
        int high = nums.length;    //注意
        while(low<high) {    //注意
            int mid = (low+high)/2;
            if (nums[mid]<=target) {    low = mid + 1;}//注意
            else if(nums[mid]>target){    high = mid;}//注意
        }
        low = (low>0 && nums[low-1]==target)?low-1:-1;
        return low;
    }
```

## 4. 二分查找细节

### 4.1 寻找一个数（基本二分搜索）

搜索一个数，如果存在，返回其索引，否则返回 -1

```java
    public int binarySearch(int[] nums, int target) {
        int left = 0;
        int right = nums.length-1;    //注意
        while(left<=right) {    //注意
            int mid = (left+right)/2;
            if(nums[mid]==target) {    return mid;}
            else if (nums[mid]>target) {    right = mid - 1;}
            else if (nums[mid]<target) {    left = mid + 1;}
        }
        return -1;
    }
```

1. 为什么 while 循环的条件中是 &lt;=，而不是 &lt; ？

    答：因为初始化 right 的赋值是 nums.length-1，即最后一个元素的索引，而不是 nums.length。

    这二者可能出现在不同功能的二分查找中，区别是：前者相当于两端都闭区间 \[left, right\]，后者相当于左闭右开区间 \[left, right\)，因为索引大小为 nums.length 是越界的。

    我们这个算法中使用的是前者 \[left, right\] 两端都闭的区间。这个区间其实就是每次进行搜索的区间，我们不妨称为「搜索区间」

2. 为什么 left = mid + 1，right = mid - 1？我看有的代码是 right = mid或者 left = mid\`，没有这些加加减减，到底怎么回事，怎么判断？

    答：这也是二分查找的一个难点，不过只要你能理解前面的内容，就能够很容易判断。

    刚才明确了「搜索区间」这个概念，而且本算法的搜索区间是两端都闭的，即 \[left, right\]。那么当我们发现索引 mid 不是要找的 target 时，如何确定下一步的搜索区间呢？

    当然是 \[left, mid - 1\] 或者 \[mid + 1, right\] 对不对？因为 mid 已经搜索过，应该从搜索区间中去除。

### 4.2 寻找左侧边界的二分搜索

```java
    public int binarySearchMin(int[] nums, int target) {
        int left = 0;
        int right = nums.length;    //注意
        while(left<right) {    //注意
            int mid = (left+right)/2;
            if (nums[mid]<target) {    left = mid + 1;}
            else if(nums[mid]>=target){    right = mid;}//注意
        }
        left = (left<nums.length && nums[left]==target)?left:-1;
        return left;
    }
```

1. 为什么 while\(left &lt; right\) 而不是 &lt;= ?

    答：用相同的方法分析，因为 right = nums.length 而不是 nums.length - 1。因此每次循环的「搜索区间」是 \[left, right\) 左闭右开。

    while\(left &lt; right\)终止的条件是 left == right，此时搜索区间 \[left, left\) 为空，所以可以正确终止。

2. 返回 -1 的操作？如果 nums 中不存在 target 这个值，怎么办？

    ```java
        while (left < right) {
            //...
        }
        // target 比所有数都大
        if (left == nums.length) return -1;
        // 类似之前算法的处理方式
        return nums[left] == target ? left : -1;
    ```

3. 为什么 left = mid + 1，right = mid ？和之前的算法不一样？

    答：这个很好解释，因为我们的「搜索区间」是 \(left, right\) 左闭右开，所以当 nums\[mid\] 被检测之后，下一步的搜索区间应该去掉 mid 分割成两个区间，即 \[left, mid\) 或 \[mid + 1, right\)。  
4. 为什么该算法能够搜索左侧边界？

    答：关键在于对于 nums\[mid\] == target 这种情况的处理：

    ```java
        if (nums[mid] == target){
            right = mid;
        }
    ```

    可见，找到 target 时不要立即返回，而是缩小「搜索区间」的上界 right，在区间 \(left, mid\)中继续搜索，即不断向左收缩，达到锁定左侧边界的目的  
5. 为什么返回 left 而不是 right？

    答：都是一样的，因为 while 终止的条件是 left == right。

### 4.3 寻找右侧边界的二分查找

```java
    public int binarySearchMax(int[] nums, int target) {
        int left = 0;
        int right = nums.length;    //注意
        while(left<right) {    //注意
            int mid = (left+right)/2;
            if (nums[mid]<=target) {    left = mid + 1;}//注意
            else if(nums[mid]>target){    right = mid;}//注意
        }
        left = (left>0 && nums[left-1]==target)?left-1:-1;
        return left;
    }
```

1. 为什么这个算法能够找到右侧边界？

    答：类似地，关键点还是这里：

    ```java
        if (nums[mid] == target) {
            left = mid + 1;
        }
    ```

2. 返回 -1 的操作？如果 nums 中不存在 target 这个值，怎么办？

    ```java
        while (left < right) {
            // ...
        }
        // target 比所有数都小
        if (left == 0) return -1;
        return nums[left-1] == target ? (left-1) : -1;
    ```



