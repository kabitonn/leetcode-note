
# 457. Circular Array Loop(M)

[]()

## 题目描述(中等)

给定一个含有正整数和负整数的**环形数组** nums。 如果某个索引中的数 k 为正数，则向前移动 k 个索引。相反，如果是负数 (-k)，则向后移动 k 个索引。因为数组是环形的，所以可以假设最后一个元素的下一个元素是第一个元素，而第一个元素的前一个元素是最后一个元素。

确定 nums 中是否存在循环（或周期）。循环必须在相同的索引处开始和结束并且循环长度 > 1。此外，一个循环中的所有运动都必须沿着同一方向进行。换句话说，一个循环中不能同时包括向前的运动和向后的运动。
 

示例 1：
```
输入：[2,-1,1,2,2]
输出：true
解释：存在循环，按索引 0 -> 2 -> 3 -> 0 。循环长度为 3 。
```

示例 2：
```
输入：[-1,2]
输出：false
解释：按索引 1 -> 1 -> 1 ... 的运动无法构成循环，因为循环的长度为 1 。根据定义，循环的长度必须大于 1 。
```

示例 3:
```
输入：[-2,1,-1,-2,-2]
输出：false
解释：按索引 1 -> 2 -> 1 -> ... 的运动无法构成循环，因为按索引 1 -> 2 的运动是向前的运动，而按索引 2 -> 1 的运动是向后的运动。一个循环中的所有运动都必须沿着同一方向进行。
```

提示：

- -1000 ≤ nums[i] ≤ 1000
- nums[i] ≠ 0
- 0 ≤ nums.length ≤ 5000
 

**进阶：**

你能写出时间时间复杂度为 O(n) 和额外空间复杂度为 O(1) 的算法吗？


## 思路

- 遍历起始节点DFS
- 快慢指针

## 解决方法

### DFS

做深度优先搜素，利用字典visited来标记已经搜素过的节点。

利用 subVisited 记录在一个方向上遇到的节点，如果新节点在 subVisited 就有环。每次选取新起点时清空。

选取下一个节点作为起点时，若在之前的DFS过程中已遍历，说明该节点不处于环中，所以可以跳过；

```java
    public boolean circularArrayLoop(int[] nums) {
        int n = nums.length;
        if (n < 2) {
            return false;
        }
        boolean[] visited = new boolean[n];
        for (int i = 0; i < n; i++) {
            if (visited[i]) {
                continue;
            }
            int cur = i;
            Set<Integer> subVisted = new HashSet<>();
            while (!visited[cur]) {
                visited[cur] = true;
                subVisted.add(cur);
                int next = (cur + nums[cur]) % n;
                next = next < 0 ? next + n : next;
                if (next == cur || nums[cur] * nums[next] < 0) {
                    break;
                }
                if (subVisted.contains(next)) {
                    return true;
                }
                cur = next;
            }
        }
        return false;
    }
```

时间复杂度： O(n)

空间复杂度： O(n)

### 快慢指针

参考环形链表的判定方法，即快慢指针。由于每一位置的值都唯一指向下一个位置，因此可以用**快慢指针**类似的方式进行解决。

一旦判断某一路径上无环，就对相关的位置原地进行标记表示已经遍历过且不符合环中元素，以避免重复运算。



```java
    public boolean circularArrayLoop1(int[] nums) {
        int n = nums.length;
        if (n < 2) {
            return false;
        }
        for (int i = 0; i < n; i++) {
            if (nums[i] == 0) {
                continue;
            }
            int slow = i;
            int fast = ((slow + nums[slow]) % n + n) % n;
            while (slow != fast && nums[fast] != 0 && nums[slow] * nums[fast] > 0) {
                slow = ((slow + nums[slow]) % n + n) % n;
                fast = ((fast + nums[fast]) % n + n) % n;
                if (nums[fast] == 0 || nums[slow] * nums[fast] < 0) {
                    break;
                }
                fast = ((fast + nums[fast]) % n + n) % n;
            }
            if (slow == fast && slow != ((slow + nums[slow]) % n + n) % n) {
                return true;
            }
            int start = i;
            while (start != fast) {
                int step = nums[start];
                nums[start] = 0;
                start = ((start + step) % n + n) % n;
            }
        }
        return false;
    }
```

时间复杂度： O(n)

空间复杂度： O(1)