
# 565. Array Nesting(M)
 
[565. 数组嵌套](https://leetcode-cn.com/problems/array-nesting/)

## 题目描述(中等)

索引从0开始长度为N的数组A，包含0到N - 1的所有整数。找到并返回最大的集合S，S[i] = {A[i], A[A[i]], A[A[A[i]]], ... }且遵守以下的规则。

假设选择索引为i的元素A[i]为S的第一个元素，S的下一个元素应该是A[A[i]]，之后是A[A[A[i]]]... 以此类推，不断添加直到S出现重复的元素。

示例 1:
```
输入: A = [5,4,0,3,1,6,2]
输出: 4
解释: 
A[0] = 5, A[1] = 4, A[2] = 0, A[3] = 3, A[4] = 1, A[5] = 6, A[6] = 2.

其中一种最长的 S[K]:
S[0] = {A[0], A[5], A[6], A[2]} = {5, 6, 2, 0}
```

**注意**:
- N是[1, 20,000]之间的整数。
- A中不含有重复的元素。
- A中的元素大小在[0, N-1]之间。


## 思路

在一个循环中，无论哪个作为首部，长度是相同的，且元素顺序也相同

## 解决方法

### 哈希集判断循环

visited 数组仅用于跟踪已经访问过的数组元素。

```java
    public int arrayNesting(int[] nums) {
        int N = nums.length;
        boolean[] visited = new boolean[N];
        int max = 0;
        for (int i = 0; i < N; i++) {
            if (visited[i]) {
                continue;
            }
            Set<Integer> set = new HashSet<>();
            int j = i;
            while (set.add(nums[j])) {
                visited[j] = true;
                j = nums[j];
            }
            max = Math.max(max, set.size());
            if (max > N / 2) {
                break;
            }
        }
        return max;
    }
```

### 标记判断循环

visited 数组仅用于跟踪已经访问过的数组元素。

```java
    public int arrayNesting1(int[] nums) {
        int N = nums.length;
        boolean[] visited = new boolean[N];
        int max = 0;
        for (int i = 0; i < N; i++) {
            if (visited[i]) {
                continue;
            }
            int j = i;
            int count = 0;
            while (!visited[j]) {
                visited[j] = true;
                j = nums[j];
                count++;
            }
            max = Math.max(max, count);
            if (max > N / 2) {
                break;
            }
        }
        return max;
    }
```

也可以不使用额外空间，在原数组中修改