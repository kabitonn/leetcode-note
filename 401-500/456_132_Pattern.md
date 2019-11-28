
# 456. 132 Pattern(M)

[456. 132模式](https://leetcode-cn.com/problems/132-pattern/)

## 题目描述(中等)

给定一个整数序列：a1, a2, ..., an，一个132模式的子序列 ai, aj, ak 被定义为：当 i < j < k 时，ai < ak < aj。设计一个算法，当给定有 n 个数字的序列时，验证这个序列中是否含有132模式的子序列。

注意：n 的值小于15000。

示例1:
```
输入: [1, 2, 3, 4]

输出: False

解释: 序列中不存在132模式的子序列。
```

示例 2:
```
输入: [3, 1, 4, 2]

输出: True

解释: 序列中有 1 个132模式的子序列： [1, 4, 2].
```

示例 3:
```
输入: [-1, 3, 2, 0]

输出: True

解释: 序列中有 3 个132模式的的子序列: [-1, 3, 2], [-1, 3, 0] 和 [-1, 2, 0].
```

## 思路

- 暴力遍历
- 贪心
- 栈

## 解决方法

### 暴力遍历

!> 超时

```java
    public boolean find132pattern(int[] nums) {
        int n = nums.length;
        for (int i = 0; i < n - 2; i++) {
            for (int j = i + 1; j < n - 1; j++) {
                for (int k = j + 1; k < n; k++) {
                    if (nums[i] < nums[k] && nums[k] < nums[j]) {
                        return true;
                    }
                }
            }
        }
        return false;
    }
```

时间复杂度：O(n^3)


### 贪心 

对每个i遍历，尽量找到最大的aj,再继续找ak;


```java
    public boolean find132pattern1(int[] nums) {
        int n = nums.length;
        for (int i = 0; i < n - 2; i++) {
            int j = i + 1;
            for (; j < n - 1; j++) {
                if (nums[j] > nums[j + 1]) {
                    break;
                }
            }
            for (int k = j + 1; k < n; k++) {
                if (nums[i] < nums[k] && nums[k] < nums[j]) {
                    return true;
                }
            }
        }
        return false;
    }
```

时间复杂度：O(n^2)

尽量找到最小的ai,然后尽量找到最大的aj,再继续找ak;

```java
    public boolean find132pattern2(int[] nums) {
        int n = nums.length;
        for (int i = 0; i < n - 2; i++) {
            for (; i < n - 2; i++) {
                if (nums[i] < nums[i + 1]) {
                    break;
                }
            }
            int j = i + 1;
            for (; j < n - 1; j++) {
                if (nums[j] > nums[j + 1]) {
                    break;
                }
            }
            for (int k = j + 1; k < n; k++) {
                if (nums[i] < nums[k] && nums[k] < nums[j]) {
                    return true;
                }
            }
        }
        return false;
    }
```

时间复杂度：O(n^2)


### 栈

首先考虑 a[i] < a[j] 的部分。当我们固定了 j 时，可以在 j 的左侧找出一个最小的数作为 a[i]，这是因为最终我们需要满足 a[i] < a[k] < a[j]，那么 a[i] 一定越小越好。因此我们可以对数组 a 维护前缀最小值，即 min[j] = min(a[1, j])，这样对于一个固定的 j，min[j] 即为最优的 a[i]；

随后我们再考虑 a[k]，其中 a[k] 需要满足 a[i] < a[k] < a[j]，即 min[j] < a[k] < a[j]。我们可以从数组 a 的末尾开始，从后向前寻找 a[k]。

可以用栈来存储所有的 a[k]。在栈中，所有候选的 a[k] 保持降序，即栈顶的元素最小，栈底的元素最大。即如果我们遇到一个新的 a[k]，那么我们会将栈顶的元素依次出栈，直到新的 a[k] 为栈中的最小元素。我们在从右向左遍历数组 a 时，假设我们当前位于 a[j]，首先我们判断是否有 nums[j] > min[j]，如果不成立，那么我们要跳过这个 a[j]，否则我们将栈顶的元素依次出栈，直到栈顶元素 stack[top] 满足 stack[top] > min[j]。在这之后，我们可以确定栈中的所有元素都大于 min[j]（即 num[i]），因此如果此时栈顶的元素和 j 可以满足 132 模式，那么我们就找到了一组合法的满足 132 模式的 i, j, k，否则我们继续寻找，此时需要把 a[j] 入栈。


```java
    public boolean find132pattern3(int[] nums) {
        int n = nums.length;
        if (n < 3) {
            return false;
        }
        int[] stack = new int[n];
        int top = 0;
        int[] min = new int[n];
        min[0] = nums[0];
        for (int i = 1; i < n; i++) {
            min[i] = Math.min(nums[i], min[i - 1]);
        }
        for (int j = n - 1; j > 0; j--) {
            if (nums[j] > min[j]) {
                while (top > 0 && stack[top - 1] <= min[j]) {
                    top--;
                }
                if (top > 0 && stack[top - 1] < nums[j]) {
                    return true;
                }
                stack[top++] = nums[j];
            }
        }
        return false;
    }
```

时间复杂度：O(n)

空间复杂度：O(n)

### 从后向前找次大值
 
从后往前遍历找到符合条件的次大值（注意只用找次大值）  
之后只需要和次大数值比较即可，如果当前元素<次大值则返回true

如果当前元素小于栈顶元素，则入栈
如果当前元素大于栈顶元素，则先出栈，出到当前元素小于栈顶元素（之前的一个局部最大值），出的同时让second和出栈元素比较，取较大的那个（临界条件）


```java
    public boolean find132pattern4(int[] nums) {
        int n = nums.length;
        if (n < 3) {
            return false;
        }
        int[] stack = new int[n];
        int top = 0;
        int secondMax = Integer.MIN_VALUE;
        for (int i = n - 1; i >= 0; i--) {
            if (nums[i] < secondMax) {
                return true;
            }
            while (top > 0 && nums[i] > stack[top - 1]) {
                secondMax = Math.max(secondMax, stack[--top]);
            }
            stack[top++] = nums[i];
        }
        return false;
    }
```

时间复杂度：O(n)

空间复杂度：O(n)