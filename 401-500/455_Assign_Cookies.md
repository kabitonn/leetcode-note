
# 455. Assign Cookies(E)

[455. 分发饼干](https://leetcode-cn.com/problems/assign-cookies/)

## 题目描述(简单)

假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。对每个孩子 i ，都有一个胃口值 `g[i]` ，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 j ，都有一个尺寸 `s[j]` 。如果 `s[j] >= g[i]` ，我们可以将这个饼干 j 分配给孩子 i ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。

**注意**：
- 你可以假设胃口值为正。
- 一个小朋友最多只能拥有一块饼干。

示例 1:
```
输入: [1,2,3], [1,1]

输出: 1

解释: 
你有三个孩子和两块小饼干，3个孩子的胃口值分别是：1,2,3。
虽然你有两块小饼干，由于他们的尺寸都是1，你只能让胃口值是1的孩子满足。
所以你应该输出1。
```

示例 2:
```
输入: [1,2], [1,2,3]

输出: 2

解释: 
你有两个孩子和三块小饼干，2个孩子的胃口值分别是1,2。
你拥有的饼干数量和尺寸都足以让所有孩子满足。
所以你应该输出2.
```

## 思路

贪心思想：
- 给一个孩子的饼干应当尽量小并且又能满足该孩子，这样大饼干才能拿来给满足度比较大的孩子。
   1. 因为满足度最小的孩子最容易得到满足，所以先满足满足度最小的孩子。
   2. 大的饼干尽量满足胃口大孩子，先分配大的饼干


## 解决方法

### 标记已分配

两数组排序，遍历到第一个符合分配需求且尚未分配的g[i],为其分配并记录；

```java
    public int findContentChildren(int[] g, int[] s) {
        boolean[] visited = new boolean[g.length];
        Arrays.sort(g);
        Arrays.sort(s);
        int count = 0;
        for (int c : s) {
            for (int i = 0; i < g.length; i++) {
                if (c >= g[i] && !visited[i]) {
                    visited[i] = true;
                    count++;
                    break;
                }
            }

        }
        return count;
    }

```

### 先满足胃口小的

两个指针标记未分配的饼干s[j]和小孩g[i],
- 若s[j] >= g[i]: 则分配
- 否则，跳过s[j]即j++，因为s[j]无法满足剩下的小孩

```java
    public int findContentChildren1(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int i = 0, j = 0;
        int m = g.length, n = s.length;
        int count = 0;
        while (i < m && j < n) {
            if (g[i] <= s[j]) {
                i++;
                count++;
            }
            j++;
        }
        return count;
    }
```

### 先分配大的饼干

两个指针标记未分配的饼干s[j]和小孩g[i],
- 若s[j] >= g[i]: 则分配
- 否则，跳过g[i]即i--，因为g[i]无法被满足

```java
    public int findContentChildren2(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int i = g.length - 1;
        int j = s.length - 1;
        int count = 0;
        while (i >= 0 && j >= 0) {
            if (g[i] <= s[j]) {
                j--;
                count++;
            }
            i--;
        }
        return count;
    }

```

