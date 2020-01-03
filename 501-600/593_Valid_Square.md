
# 593. Valid Square(M)
 
[593. 有效的正方形](https://leetcode-cn.com/problems/valid-square/)

## 题目描述(中等)

给定二维空间中四点的坐标，返回四点是否可以构造一个正方形。

一个点的坐标（x，y）由一个有两个整数的整数数组表示。

示例:
```
输入: p1 = [0,0], p2 = [1,1], p3 = [1,0], p4 = [0,1]
输出: True
```

**注意**:
- 所有输入整数都在 [-10000，10000] 范围内。
- 一个有效的正方形有四个等长的正长和四个等角（90度角）。
- 输入点没有顺序。


## 思路

四条边相等，两条对角线相等

## 解决方法

### 边排序判断相等

边长相等且边长小于对角线且对角线相等

```java
    public boolean validSquare(int[] p1, int[] p2, int[] p3, int[] p4) {
        int[] edge = new int[6];
        int count = 0;
        edge[count++] = getDistance(p1, p2);
        edge[count++] = getDistance(p1, p3);
        edge[count++] = getDistance(p1, p4);
        edge[count++] = getDistance(p2, p3);
        edge[count++] = getDistance(p2, p4);
        edge[count++] = getDistance(p3, p4);
        Arrays.sort(edge);
        if (edge[0] != 0 && edge[0] == edge[1] && edge[0] == edge[2] && edge[0] == edge[3]) {
            if (edge[4] == edge[5]) {
                return true;
            }
        }
        return false;
    }

    public int getDistance(int[] p1, int[] p2) {
        int dx = p2[0] - p1[0];
        int dy = p2[1] - p1[1];
        return dx * dx + dy * dy;
    }

```

### 遍历三种情况

四个点六条边组成四边形的情况只有三种，确定一条对角线后即确定图形的所有边，然后判断边长相等和对角线相等

```java
    public int getDistance(int[] p1, int[] p2) {
        int dx = p2[0] - p1[0];
        int dy = p2[1] - p1[1];
        return dx * dx + dy * dy;
    }

    public boolean validSquare1(int[] p1, int[] p2, int[] p3, int[] p4) {
        int p12 = getDistance(p1, p2);
        int p13 = getDistance(p1, p3);
        int p14 = getDistance(p1, p4);
        int p23 = getDistance(p2, p3);
        int p24 = getDistance(p2, p4);
        int p34 = getDistance(p3, p4);
        if (p12 == 0 || p13 == 0 || p14 == 0 || p23 == 0 || p24 == 0 || p34 == 0) {
            return false;
        }
        if ((p12 == p34 && p13 == p14 && p13 == p23 && p13 == p24) ||
            (p13 == p24 && p12 == p14 && p12 == p23 && p12 == p34) ||
            (p14 == p23 && p12 == p13 && p12 == p24 && p12 == p34)) {
            return true;
        }
        return false;
    }
```