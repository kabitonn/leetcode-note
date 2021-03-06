# 134. Gas Station\(M\)

[134. 加油站](https://leetcode-cn.com/problems/gas-station/)

## 题目描述\(中等\)

There are N gas stations along a circular route, where the amount of gas at station i is `gas[i]`.

You have a car with an unlimited gas tank and it costs `cost[i]` of gas to travel from station i to its next station \(i+1\). You begin the journey with an empty tank at one of the gas stations.

Return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return -1.

**Note**:

* If there exists a solution, it is guaranteed to be unique.
* Both input arrays are non-empty and have the same length.
* Each element in the input arrays is a non-negative integer.

Example 1:

```
Input: 
gas  = [1,2,3,4,5]
cost = [3,4,5,1,2]

Output: 3

Explanation:
Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 4. Your tank = 4 - 1 + 5 = 8
Travel to station 0. Your tank = 8 - 2 + 1 = 7
Travel to station 1. Your tank = 7 - 3 + 2 = 6
Travel to station 2. Your tank = 6 - 4 + 3 = 5
Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3.
Therefore, return 3 as the starting index.
```

Example 2:

```
Input: 
gas  = [2,3,4]
cost = [3,4,3]

Output: -1

Explanation:
You can't start at station 0 or 1, as there is not enough gas to travel to the next station.
Let's start at station 2 and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 0. Your tank = 4 - 3 + 2 = 3
Travel to station 1. Your tank = 3 - 3 + 3 = 3
You cannot travel back to station 2, as it requires 4 unit of gas but you only have 3.
Therefore, you can't travel around the circuit once no matter where you start.
```

## 

## 思路

![](../assets/leetcode-note/101-200/134-t-1.png)

* ## 解决方法

### 遍历

考虑从第`0`个点出发，能否回到第`0`个点。

考虑从第`1`个点出发，能否回到第`1`个点。

考虑从第`2`个点出发，能否回到第`2`个点。

... ...

考虑从第`n`个点出发，能否回到第`n`个点。

```java
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int len = gas.length;
        int start = -1;
        for (int i = 0; i < len; i++) {
            int remain = 0;
            for (int j = 0; j < len; j++) {
                int k = (i + j) % len;
                remain += (gas[k] - cost[k]);
                if (remain < 0) {
                    break;
                }
            }
            if (remain >= 0) {
                start = i;
                break;
            }
        }
        return start;
    }
```

时间复杂度：O(n^2)
空间复杂度：O(1)




### 遍历优化
```
* * * * * *
^     ^
i     j
```
当考虑 i 能到达的最远的时候，假设是 j。

那么 i + 1 到 j 之间的节点是不是就都不可能绕一圈了？

假设 i + 1 的节点能绕一圈，那么就意味着从 i + 1 开始一定能到达 j + 1。

又因为从 i 能到达 i + 1，所以从 i 也能到达 j + 1。

但事实上，i 最远到达 j 。产生矛盾，所以 i + 1 的节点一定不能绕一圈。同理，其他的也是一样的证明。

所以下一次的 i 我们不需要从 i + 1 开始考虑，直接从 j + 1 开始考虑即可。

```
* * * * * *
  ^   ^
  j   i

```
如果 i 最远能够到达 j ，根据上边的结论 i + 1 到 j 之间的节点都不可能绕一圈了。想象成一个圆，所以 i 后边的节点就都不需要考虑了，直接返回 -1 即可。

```java
    public int canCompleteCircuit1(int[] gas, int[] cost) {
        int len = gas.length;
        for (int i = 0; i < len; i++) {
            int j = i;
            int remain = gas[j];
            while (remain - cost[j] >= 0) {
                remain -= cost[j];
                j = (j + 1) % len;
                remain += gas[j];
                if (j == i) {
                    return i;
                }
            }
            if (j < i) {
                return -1;
            }
            i = j;
        }
        return -1;
    }
```
时间复杂度：O(n)
空间复杂度：O(1)



### 贪心 一次遍历

如果无法环绕一周，那么gas[i]-cost[i]的差值累加必然小于0,；否则，就是能够环绕一周。
一次遍历，在遍历中一边累加total一边寻找正确的start。


```java
    public int canCompleteCircuit2(int[] gas, int[] cost) {
        int n = gas.length;

        int totalTank = 0;
        int currTank = 0;
        int start = 0;
        for (int i = 0; i < n; ++i) {
            totalTank += gas[i] - cost[i];
            currTank += gas[i] - cost[i];
            // If one couldn't get here,
            if (currTank < 0) {
                // Pick up the next station as the starting one.
                start = i + 1;
                // Start with an empty tank.
                currTank = 0;
            }
        }
        return totalTank >= 0 ? start : -1;
    }
```

时间复杂度：O(n)
空间复杂度：O(1)



