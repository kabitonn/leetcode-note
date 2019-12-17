
# 496. Next Greater Element I(E)

[496. 下一个更大元素 I](https://leetcode-cn.com/problems/next-greater-element-i/)

## 题目描述(中等)

给定两个没有重复元素的数组 nums1 和 nums2 ，其中nums1 是 nums2 的子集。找到 nums1 中每个元素在 nums2 中的下一个比其大的值。

nums1 中数字 x 的下一个更大元素是指 x 在 nums2 中对应位置的右边的第一个比 x 大的元素。如果不存在，对应位置输出-1。

示例 1:
```
输入: nums1 = [4,1,2], nums2 = [1,3,4,2].
输出: [-1,3,-1]
解释:
    对于num1中的数字4，你无法在第二个数组中找到下一个更大的数字，因此输出 -1。
    对于num1中的数字1，第二个数组中数字1右边的下一个较大数字是 3。
    对于num1中的数字2，第二个数组中没有下一个更大的数字，因此输出 -1。
```

示例 2:
```
输入: nums1 = [2,4], nums2 = [1,2,3,4].
输出: [3,-1]
解释:
    对于num1中的数字2，第二个数组中的下一个较大数字是3。
    对于num1中的数字4，第二个数组中没有下一个更大的数字，因此输出 -1。
```

**注意**:
- nums1和nums2中所有元素是唯一的。
- nums1和nums2 的数组大小都不超过1000。

## 思路

- 暴力遍历
- 索引
- 哈希
- 单调栈

## 解决方法

### 遍历

nums1中的数，在nums2中遍历在找到该数的后面判断有没有更大的值

```java
        public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        int n1 = nums1.length, n2 = nums2.length;
        int[] next = new int[n1];
        for (int i = 0; i < n1; i++) {
            next[i] = -1;
            boolean found = false;
            for (int j = 0; j < n2; j++) {
                if (!found && nums1[i] == nums2[j]) {
                    found = true;
                } else if (found && nums1[i] < nums2[j]) {
                    next[i] = nums2[j];
                    break;
                }
            }
        }
        return next;
    }
```

时间复杂度：O(m*n)

空间复杂度：O(m)

### 索引

提前遍历，记录nums1中的数在nums2的索引

```java
        public int[] nextGreaterElement_1(int[] nums1, int[] nums2) {
        int n1 = nums1.length, n2 = nums2.length;
        int[] next = new int[n1];
        int[] map = new int[n1];
        for (int i = 0; i < n1; i++) {
            for (int j = 0; j < n2; j++) {
                if (nums1[i] == nums2[j]) {
                    map[i] = j;
                    break;
                }
            }
        }
        for (int i = 0; i < n1; i++) {
            next[i] = -1;
            for (int j = map[i] + 1; j < n2; j++) {
                if (nums1[i] < nums2[j]) {
                    next[i] = nums2[j];
                    break;
                }
            }
        }
        return next;
    }
```
时间复杂度：O(m*n)

空间复杂度：O(m)

### 桶实现索引

```java
    public int[] nextGreaterElement3(int[] nums1, int[] nums2) {
        int n1 = nums1.length, n2 = nums2.length;
        int[] next = new int[n1];
        int max = 0;
        for (int num : nums2) {
            max = Math.max(num, max);
        }
        int[] map = new int[max + 1];
        for (int i = 0; i < n2; i++) {
            map[nums2[i]] = i;
        }
        for (int i = 0; i < n1; i++) {
            next[i] = -1;
            for (int j = map[nums1[i]] + 1; j < n2; j++) {
                if (nums1[i] < nums2[j]) {
                    next[i] = nums2[j];
                    break;
                }
            }
        }
        return next;
    }
```
时间复杂度：O(n)

空间复杂度：O(max(nums2))，nums2中最大的数值

### 在母集中遍历比自己大的后者

遍历nums2，以数值为key，在自己后面的更大的数值为value，遍历nums1时，若key存在说明在后面有更大的值，key不存在则说明后面没有更大的值

```java
    public int[] nextGreaterElement1(int[] nums1, int[] nums2) {
        int n1 = nums1.length, n2 = nums2.length;
        int[] next = new int[n1];
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < n2; i++) {
            for (int j = i + 1; j < n2; j++) {
                if (nums2[i] < nums2[j]) {
                    map.put(nums2[i], nums2[j]);
                    break;
                }
            }
        }
        for (int i = 0; i < n1; i++) {
            next[i] = map.getOrDefault(nums1[i], -1);
        }
        return next;
    }
```

时间复杂度：O(m*n)

空间复杂度：O(m)

### 单调栈

维护一个单调栈，栈中的元素从栈顶到栈底是单调不降的。当我们遇到一个新的元素 nums2[i] 时，我们判断栈顶元素是否小于 nums2[i]，如果是，那么栈顶元素的下一个更大元素即为 nums2[i]，我们将栈顶元素出栈。重复这一操作，直到栈为空或者栈顶元素大于 nums2[i]。此时我们将 nums2[i] 入栈，保持栈的单调性，并对接下来的 nums2[i + 1], nums2[i + 2] ... 执行同样的操作。



```java
    public int[] nextGreaterElement2(int[] nums1, int[] nums2) {
        int n1 = nums1.length, n2 = nums2.length;
        int[] next = new int[n1];
        int[] stack = new int[n2];
        Map<Integer, Integer> map = new HashMap<>();
        int top = 0;
        for (int num : nums2) {
            while (top > 0 && stack[top - 1] < num) {
                map.put(stack[--top], num);
            }
            stack[top++] = num;
        }
        while (top > 0) {
            map.put(stack[--top], -1);
        }
        for (int i = 0; i < n1; i++) {
            next[i] = map.get(nums1[i]);
        }
        return next;
    }

```

时间复杂度：O(m+n)

空间复杂度：O(m)