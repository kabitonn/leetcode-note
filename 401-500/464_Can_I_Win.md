# 464. Can I Win(M)


[464. 我能赢吗](https://leetcode-cn.com/problems/can-i-win/)

## 题目描述(中等)

在 "100 game" 这个游戏中，两名玩家轮流选择从 1 到 10 的任意整数，累计整数和，先使得累计整数和达到 100 的玩家，即为胜者。

如果我们将游戏规则改为 “玩家不能重复使用整数” 呢？

例如，两个玩家可以轮流从公共整数池中抽取从 1 到 15 的整数（不放回），直到累计整数和 >= 100。

给定一个整数 maxChoosableInteger （整数池中可选择的最大数）和另一个整数 desiredTotal（累计和），判断先出手的玩家是否能稳赢（假设两位玩家游戏时都表现最佳）？

你可以假设 `maxChoosableInteger` 不会大于 20， `desiredTotal` 不会大于 300。

示例：
```
输入：
maxChoosableInteger = 10
desiredTotal = 11

输出：
false

解释：
无论第一个玩家选择哪个整数，他都会失败。
第一个玩家可以选择从 1 到 10 的整数。
如果第一个玩家选择 1，那么第二个玩家只能选择从 2 到 10 的整数。
第二个玩家可以通过选择整数 10（那么累积和为 11 >= desiredTotal），从而取得胜利.
同样地，第一个玩家选择任意其他整数，第二个玩家都会赢。
```

## 思路

- DFS 递归

## 解决方法

### DFS 

1. 对弈的两个人都是高手，所以他俩的思考方式一定是一样的，因此对弈的过程，也可以看作是递归的过程，每次走步都为了更接近让自己赢得胜利的目标
2. 稳赢的思考过程如下:
   1. 在可选的数字中，我用最大的数字可以触及目标，那么自己就赢了
   2. 自己怎么选都不可能触及目标的话，那么就寄希望于自己选择一个对方无法赢的数字，只要对方赢不了，那么就是自己赢了


!> 超时

```java
    public boolean canIWin0(int maxChoosableInteger, int desiredTotal) {
        int canReachTotal = (1 + maxChoosableInteger) * maxChoosableInteger / 2;
        if (canReachTotal < desiredTotal) {
            return false;
        } else if (canReachTotal == desiredTotal) {
            return (maxChoosableInteger & 1) == 1;
        }
        return dfs(maxChoosableInteger, desiredTotal, new boolean[maxChoosableInteger + 1]);
    }

    public boolean dfs(int maxChoosableInteger, int desiredTotal, boolean[] visited) {
        for (int i = maxChoosableInteger; i > 0; i--) {
            if (visited[i]) {
                continue;
            }
            visited[i] = true;
            if (i >= desiredTotal || !dfs(maxChoosableInteger, desiredTotal - i, visited)) {
                visited[i] = false;
                return true;
            }
            visited[i] = false;
        }
        return false;
    }
```

### DFS 递归记忆化

递归记忆化，无法采用数组记录状态，由于 maxChoosableInteger <= 20, 所以可以采用 int 的 bit 位表示状态

```java
    public boolean canIWin2(int maxChoosableInteger, int desiredTotal) {
        int canReachTotal = (1 + maxChoosableInteger) * maxChoosableInteger / 2;
        if (canReachTotal < desiredTotal) {
            return false;
        } else if (canReachTotal == desiredTotal) {
            return (maxChoosableInteger & 1) == 1;
        }
        return dfs(maxChoosableInteger, desiredTotal, 0, new Integer[1 << maxChoosableInteger + 1]);
    }

    public boolean dfs(int maxChoosableInteger, int desiredTotal, int status, Integer[] memo) {
        if (memo[status] != null) {
            return memo[status] == 1;
        }
        for (int i = maxChoosableInteger; i > 0; i--) {
            if (((status >> i) & 1) == 1) {
                continue;
            }
            if (i >= desiredTotal || !dfs(maxChoosableInteger, desiredTotal - i, status | (1 << i), memo)) {
                memo[status] = 1;
                return true;
            }
        }
        memo[status] = 0;
        return false;
    }
```