# 313. Super Ugly Number(M)


[313. 超级丑数](https://leetcode-cn.com/problems/super-ugly-number/)

## 题目描述(中等)

编写一段程序来查找第 n 个超级丑数。

超级丑数是指其所有质因数都是长度为 k 的质数列表 primes 中的正整数。

示例:
```
输入: n = 12, primes = [2,7,13,19]
输出: 32 
解释: 给定长度为 4 的质数列表 primes = [2,7,13,19]，前 12 个超级丑数序列为：[1,2,4,7,8,13,14,16,19,26,28,32] 。
```
**说明**:
- 1 是任何给定 primes 的超级丑数。
- 给定 primes 中的数字以升序排列。
- 0 < k ≤ 100, 0 < n ≤ 106, 0 < primes[i] < 1000 。
- 第 n 个超级丑数确保在 32 位有符整数范围内。


## 思路

## 解决方法

### 动态规划

$$ dp[i] = min(dp[point[p]] * primes[p] for p in [0,primes.length]) $$

多个指针指向仍能被继续使用的最后一个相对应的数，当前生成的丑数若大于该指针指向的数所能生成的下一个丑数，即代表该指针不再继续使用，应指向下一个数；


- 我们每次从多组新丑数中，挑选大于已知末尾丑数的最小值，来作为新的丑数
- 新丑数，如果某次未被选中放入数轴，那么肯定有某一刻会被放进去
- 所以使用多个个游标来确定已被放入数轴的新丑数最小值，每次仅需要对比这多个游标与后序新丑数即可

```java
    public int nthSuperUglyNumber(int n, int[] primes) {
        int k = primes.length;
        int[] point = new int[k];
        int[] dp = new int[n];
        dp[0] = 1;
        for (int i = 1; i < n; i++) {
            int min = Integer.MAX_VALUE;
            for (int p = 0; p < primes.length; p++) {
                min = Math.min(dp[point[p]] * primes[p], min);
            }
            dp[i] = min;
            for (int p = 0; p < primes.length; p++) {
                if (min >= dp[point[p]] * primes[p]) {
                    point[p]++;
                }
            }
        }
        return dp[n - 1];
    }
```

### 最小堆

因为丑数是primes中数的倍数，我们不断把它们的倍数压入栈中，再按顺序弹出n次，对于相同的丑数即刻弹出

```java
    public int nthSuperUglyNumber1(int n, int[] primes) {
        PriorityQueue<Long> minHeap = new PriorityQueue<>();
        minHeap.add((long) 1);
        long uglyNum = 1;
        long[] values = Arrays.stream(primes).asLongStream().toArray();
        for (int i = 0; i < n; i++) {
            uglyNum = minHeap.poll();
            while (!minHeap.isEmpty() && uglyNum == minHeap.peek()) {
                minHeap.poll();
            }
            for (long v : values) {
                minHeap.add(v * uglyNum);
            }
        }
        return (int) uglyNum;
    }
```