
# 503. Next Greater Element II(M)

[503. 下一个更大元素 II](https://leetcode-cn.com/problems/next-greater-element-ii/)

## 题目描述(中等)

给定一个循环数组（最后一个元素的下一个元素是数组的第一个元素），输出每个元素的下一个更大元素。数字 x 的下一个更大的元素是按数组遍历顺序，这个数字之后的第一个比它更大的数，这意味着你应该循环地搜索它的下一个更大的数。如果不存在，则输出 -1。

示例 1:
```
输入: [1,2,1]
输出: [2,-1,2]
解释: 第一个 1 的下一个更大的数是 2；
数字 2 找不到下一个更大的数； 
第二个 1 的下一个最大的数需要循环搜索，结果也是 2。
```

**注意**: 输入数组的长度不会超过 10000。

## 思路

每个数字都需要确保向后寻找更大值时遍历数组的一圈

- 暴力遍历
- 单调栈

## 解决方法

### 暴力遍历

对于每一个数字，向后循环遍历寻找更大的值

```java
    public int[] nextGreaterElements(int[] nums) {
        int n = nums.length;
        int[] next = new int[n];
        for (int i = 0; i < n; i++) {
            next[i] = -1;
            for (int j = (i + 1) % n; j != i; j = (j + 1) % n) {
                if (nums[i] < nums[j]) {
                    next[i] = nums[j];
                    break;
                }
            }
        }
        return next;
    }
```

### 最大元素开始遍历 单调栈

首先找到数组中最大元素的索引，由于该值向后循环没有更大的值，从该位置起循环遍历一圈即可，

利用单调栈，自栈顶向栈底单调不降，循环遍历过程中，若当前元素大于栈顶元素，说明栈顶元素有更大的值，从栈顶弹出，循环判断栈顶元素是否能弹出；最后处于栈中的数字不存在比其更大的值

```java
    public int[] nextGreaterElements1(int[] nums) {
        int max = Integer.MIN_VALUE;
        int maxIndex = -1;
        int n = nums.length;
        int[] next = new int[n];
        if (n == 0) {
            return next;
        }
        for (int i = 0; i < n; i++) {
            if (nums[i] > max) {
                maxIndex = i;
                max = nums[i];
            }
        }
        int[] stackIndex = new int[n];
        int top = 0;
        for (int k = maxIndex + 1; k <= maxIndex + n; k++) {
            int i = k % n;
            while (top > 0 && nums[stackIndex[top - 1]] < nums[i]) {
                next[stackIndex[--top]] = nums[i];
            }
            stackIndex[top++] = i;
        }
        while (top > 0) {
            next[stackIndex[--top]] = -1;
        }
        return next;
    }
```


### 单调栈 两轮循环

由于是循环数组，那么我们只需要循环两遍,下标移动范围 [0, 2*n-1]

```java
    public int[] nextGreaterElements2(int[] nums) {
        int n = nums.length;
        int[] next = new int[n];
        if (n == 0) {
            return next;
        }
        int[] stackIndex = new int[n];
        int top = 0;
        for (int k = 0; k < n * 2; k++) {
            int i = k % n;
            while (top > 0 && nums[stackIndex[top - 1]] < nums[i]) {
                next[stackIndex[--top]] = nums[i];
            }
            //超出 n 的那部分，无需再次入栈,遍历第 2 遍，是为了让栈弹出，即为更大值存在于其前面的元素
            if (k < n) {
                stackIndex[top++] = i;
            }
        }
        while (top > 0) {
            next[stackIndex[--top]] = -1;
        }
        return next;
    }
```


### 倒序遍历

维护一个单调栈，自栈顶向栈底单调上升，自后向前遍历，若大于等于栈顶元素，说明栈顶元素无法满足该元素而出栈，直到栈为空或小于栈顶元素，若当前元素小于栈顶元素，说明该元素存在更大值；

```java
    public int[] nextGreaterElements3(int[] nums) {
        int n = nums.length;
        int[] next = new int[n];
        int[] stackIndex = new int[n];
        int top = 0;
        for (int k = 2 * n - 1; k >= 0; k--) {
            int i = k % n;
            while (top > 0 && nums[stackIndex[top - 1]] <= nums[i]) {
                top--;
            }
            next[i] = top > 0 ? nums[stackIndex[top - 1]] : -1;
            stackIndex[top++] = i;
        }
        return next;
    }
```
