
# 526. Beautiful Arrangement(M)

[526. 优美的排列](https://leetcode-cn.com/problems/beautiful-arrangement/)

## 题目描述(中等)

假设有从 1 到 N 的 N 个整数，如果从这 N 个数字中成功构造出一个数组，使得数组的第 i 位 (1 <= i <= N) 满足如下两个条件中的一个，我们就称这个数组为一个优美的排列。条件：

- 第 i 位的数字能被 i 整除
- i 能被第 i 位上的数字整除

现在给定一个整数 N，请问可以构造多少个优美的排列？

示例1:
```
输入: 2
输出: 2
解释: 

第 1 个优美的排列是 [1, 2]:
  第 1 个位置（i=1）上的数字是1，1能被 i（i=1）整除
  第 2 个位置（i=2）上的数字是2，2能被 i（i=2）整除

第 2 个优美的排列是 [2, 1]:
  第 1 个位置（i=1）上的数字是2，2能被 i（i=1）整除
  第 2 个位置（i=2）上的数字是1，i（i=2）能被 1 整除
```

**说明**:
- N 是一个正整数，并且不会超过15。


## 思路

- 排列数的判断

## 解决方法

### 回溯生成排列

每调用一层就进入一个 for 循环，相当于列出了所有解，然后挑选了我们需要的。其实本质上就是深度优先遍历 DFS。

visited记录是否使用过(数组或二进制位来实现)

```java
    public int countArrangement(int N) {
        return backtrack(N, 1, 0);
    }

    public int backtrack(int N, int pos, int visited) {
        if (pos > N) {
            return 1;
        }
        int count = 0;
        for (int i = 1; i <= N; i++) {
            if ((1 << i & visited) != 0) {
                continue;
            }
            if (pos % i != 0 && i % pos != 0) {
                continue;
            }
            visited ^= 1 << i;
            count += backtrack(N, pos + 1, visited);
            visited ^= 1 << i;
        }
        return count;
    }

```


### 数组交换生成排列

只需要用一个 for 循环，把每一个数字都放到 pos 一次

```java
        public int countArrangement1(int N) {
        int[] nums = new int[N + 1];
        for (int i = 1; i <= N; i++) {
            nums[i] = i;
        }
        return arrange(nums, 1);
    }

    public int arrange(int[] nums, int pos) {
        if (pos == nums.length) {
            return 1;
        }
        int count = 0;
        for (int i = pos; i < nums.length; i++) {
            if (nums[i] % pos == 0 || pos % nums[i] == 0) {
                swap(nums, i, pos);
                count += arrange(nums, pos + 1);
                swap(nums, i, pos);
            }
        }
        return count;
    }

    public void swap(int[] nums, int i, int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
```