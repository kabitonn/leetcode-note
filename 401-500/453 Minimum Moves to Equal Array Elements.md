
# 453. Minimum Moves to Equal Array Elements(E)

[453. 最小移动次数使数组元素相等](https://leetcode-cn.com/problems/minimum-moves-to-equal-array-elements/)

## 题目描述(简单)

给定一个长度为 n 的非空整数数组，找到让数组所有元素相等的最小移动次数。每次移动可以使 n - 1 个元素增加 1。

示例:
```
输入:
[1,2,3]

输出:
3

解释:
只需要3次移动（注意每次移动会增加两个元素的值）：

[1,2,3]  =>  [2,3,3]  =>  [3,4,3]  =>  [4,4,4]
```

## 思路

每次移动可以使 n - 1 个元素增加 1，等价于使一个元素减少 1 ；
- 找最小值

## 解决方法


### 暴力法 模拟

!> 超时

为了在最小移动内使所有元素相等，我们需要在数组的最大元素之外的所有元素中执行增加。在暴力法中，扫描整个数组以查找最大值和最小元素。此后，我们将 1 添加到除最大元素之外的所有元素，并增加移动数的计数。同样，重复相同的过程，直到最大元素和最小元素彼此相等。


```java
    public int minMoves0(int[] nums) {
        int count = 0;
        int min = 0, max = nums.length - 1;
        while (true) {
            for (int i = 0; i < nums.length; i++) {
                if (nums[max] < nums[i]) {
                    max = i;
                }
                if (nums[min] > nums[i]) {
                    min = i;
                }
            }
            int diff = nums[max] - nums[min];
            if (diff == 0) {
                break;
            }
            count += diff;
            for (int i = 0; i < nums.length; i++) {
                if (i != max) {
                    nums[i] = nums[i] + diff;
                }
            }
        }
        return count;
    }
```
时间复杂度：O(n^2)

空间复杂度：O(1)

### 排序 统计

上述方法中diff=max-min;可以利用数组的有序性在 O(1) 时间内找到更新后的最大值和最小值  
在每一步用最大最小值差更新数组  
在更新元素之后，我们要登记的 diff 差值也不变，因为 max 和 min 增加的数字相同

```java
    public int minMoves1(int[] nums) {
        int count = 0;
        Arrays.sort(nums);
        for (int i = nums.length - 1; i > 0; i--) {
            count += nums[i] - nums[0];
        }
        return count;
    }
```

时间复杂度：O(nlog(n))

### 动态规划

不考虑整个问题，而是将问题分解。假设，直到 i-1 位置的元素都已经相等，我们只需要考虑 i 位的元素，将差值 diff=a[i]-a[i-1] 加到总移动次数上，使得第 i 位也相等。moves=moves+diff。


```java
    public int minMoves2(int[] nums) {
        int count = 0;
        Arrays.sort(nums);
        for (int i = 1; i < nums.length; i++) {
            int diff = count + nums[i] - nums[i - 1];
            nums[i] += count;
            count += diff;
        }
        return count;
    }
```

时间复杂度：O(nlog(n))

### 数学法

将除了一个元素之外的全部元素+1，等价于将该元素-1，因为我们只对元素的相对大小感兴趣。因此，该问题简化为需要进行的减法次数。

```java
    public int minMoves3(int[] nums) {
        int min = Integer.MAX_VALUE;
        int sum = 0;
        for (int n : nums) {
            if (n < min) {
                min = n;
            }
            sum += n;
        }
        int count = sum - min * nums.length;
        return count;

    }
```
时间复杂度：O(n)。对数组进行了一次遍历。

空间复杂度：O(1)。不需要额外空间。

上述sum(a[i])可能会非常大，造成整数越界

```java
    public int minMoves4(int[] nums) {
        int min = Integer.MAX_VALUE;
        int count = 0;
        for (int n : nums) {
            min = Math.min(min, n);
        }
        for (int n : nums) {
            count += n - min;
        }
        return count;
    }
```
时间复杂度：O(n)。一次遍历寻找最小值，一次遍历计算次数。

空间复杂度：O(1)。不需要额外空间。