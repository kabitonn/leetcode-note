# 264. Ugly Number II(M)

[264. 丑数 II](https://leetcode-cn.com/problems/ugly-number-ii/)

## 题目描述(中等)

Write a program to find the n-th ugly number.

Ugly numbers are positive numbers whose prime factors only include 2, 3, 5. 

Example:
```
Input: n = 10
Output: 12
Explanation: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 is the sequence of the first 10 ugly numbers.
```
**Note**:  

- 1 is typically treated as an ugly number.
- n does not exceed 1690.



## 思路

- 动态规划
- 最小堆

## 解决方法

### 动态规划 三指针

$$ dp[i] = min(2 * dp[p_2], 3 * dp[p_3], 5 * dp[p_5]) $$

三指针指向仍能被继续使用的最后一个相对应的数，当前生成的丑数若大于该指针指向的数所能生成的下一个丑数，即代表该指针不再继续使用，应指向下一个数；


- 我们每次从三组新丑数中，挑选大于已知末尾丑数的最小值，来作为新的丑数
- 乘3或者乘5的新丑数，如果某次未被选中放入数轴，那么肯定有某一刻会被放进去
- 所以使用三个游标来确定已被放入数轴的新丑数最小值，每次仅需要对比这三个游标与2，3，5乘积即可

```java
    public int nthUglyNumber(int n) {
        int[] dp = new int[n];
        dp[0] = 1;
        //三指针指向仍能被继续使用的最后一个相对应的数
        int p2 = 0, p3 = 0, p5 = 0;
        int v2 = 0, v3 = 0, v5 = 0;
        for (int i = 1; i < n; i++) {
            v2 = dp[p2] * 2;
            v3 = dp[p3] * 3;
            v5 = dp[p5] * 5;
            dp[i] = Math.min(Math.min(v2, v3), v5);
            if (dp[i] >= v2) {
                p2++;
            }
            if (dp[i] >= v3) {
                p3++;
            }
            if (dp[i] >= v5) {
                p5++;
            }
        }
        return dp[n - 1];
    }

```

### 最小堆

因为丑数是2, 3, 5的倍数，我们不断把它们的倍数压入栈中，再按顺序弹出n次

```java
    public int nthUglyNumber1(int n) {
        PriorityQueue<Long> minHeap = new PriorityQueue<>();
        minHeap.add((long)1);
        long uglyNum = 1;
        long[] values = {2, 3, 5};
        for (int i = 0; i < n; i++) {
            uglyNum = minHeap.poll();
            while (!minHeap.isEmpty()&&uglyNum == minHeap.peek()) {
                minHeap.poll();
            }
            for (long v : values) {
                // int 乘积会溢出，出现在堆顶
                minHeap.add(v * uglyNum);
            }
        }
        return (int)uglyNum;
    }
```

