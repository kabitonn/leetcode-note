# 292. Nim Game
[292. Nim Game](https://leetcode-cn.com/problems/nim-game/)

## 题目描述(简单)

You are playing the following Nim Game with your friend: There is a heap of stones on the table, each time one of you take turns to remove 1 to 3 stones. The one who removes the last stone will be the winner. You will take the first turn to remove the stones.

Both of you are very clever and have optimal strategies for the game. Write a function to determine whether you can win the game given the number of stones in the heap.

Example:
```
Input: 4
Output: false 
Explanation: If there are 4 stones in the heap, then you will never win the game;
             No matter 1, 2, or 3 stones you remove, the last stone will always be 
             removed by your friend.
```

## 思路

## 解决方法

### 规律

如果堆中石头的数量 n 不能被 4 整除，那么你总是可以赢得 Nim 游戏的胜利
第一次拿完使剩余石头是4的倍数即可获胜

```java
    public boolean canWinNim(int n) {
    	return n%4!=0;
    }
```
