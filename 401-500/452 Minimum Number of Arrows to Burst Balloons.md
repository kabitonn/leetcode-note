
# 452. Minimum Number of Arrows to Burst Balloons(M)

[452. 用最少数量的箭引爆气球](https://leetcode-cn.com/problems/minimum-number-of-arrows-to-burst-balloons/)

## 题目描述(中等)

在二维空间中有许多球形的气球。对于每个气球，提供的输入是水平方向上，气球直径的开始和结束坐标。由于它是水平的，所以y坐标并不重要，因此只要知道开始和结束的x坐标就足够了。开始坐标总是小于结束坐标。平面内最多存在10^4个气球。

一支弓箭可以沿着x轴从不同点完全垂直地射出。在坐标x处射出一支箭，若有一个气球的直径的开始和结束坐标为 $x_{start}, x_{end}$， 且满足  $x_{start} ≤ x ≤ x_{end}$，则该气球会被引爆。可以射出的弓箭的数量没有限制。 弓箭一旦被射出之后，可以无限地前进。我们想找到使得所有气球全部被引爆，所需的弓箭的最小数量。

Example:
```
输入:
[[10,16], [2,8], [1,6], [7,12]]

输出:
2

解释:
对于该样例，我们可以在x = 6（射爆[2,8],[1,6]两个气球）和 x = 11（射爆另外两个气球）。
```

## 思路

- 排序 贪心

## 解决方法

### 起点升序 判断重叠



right:在遍历的过程中使用当前这只箭能够击穿所有气球的最远距离  
若后序和当前这支箭的射程有重叠，则使用当前这支箭向后推进，区间的末尾端点很重要：如果不使用新的箭，新区间末尾端点就要和当前的“最远距离”作一个比较，取最小值

```java
    public int findMinArrowShots(int[][] points) {
        Arrays.sort(points, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                return o1[0] != o2[0] ? o1[0] - o2[0] : o1[1] - o2[1];
            }
        });
        int n = points.length;
        int i = 0;
        int count = 0;
        while (i < n) {
            int j = i + 1;
            int right = points[i][1];
            while (j < n && right >= points[j][0]) {
                right = Math.min(right, points[j][1]);
                j++;
            }
            i = j;
            count++;
        }
        return count;
    }
```

right:在遍历的过程中使用当前这只箭能够击穿所有气球的最远距离

```java
    public int findMinArrowShots1(int[][] points) {
        if (points.length == 0) {
            return 0;
        }
        Arrays.sort(points, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                return o1[0] - o2[0];
            }
        });
        int n = points.length;
        int count = 1;
        int right = points[0][1];
        for (int i = 1; i < n; i++) {
            if (points[i][0] <= right) {
                right = Math.min(right, points[i][1]);
            } else {
                count++;
                right = points[i][1];
            }
        }
        return count;
    }

```

时间复杂度：O(nlog(n))

### 终点升序 判断重叠


```java
    public int findMinArrowShots2(int[][] points) {
        if (points.length == 0) {
            return 0;
        }
        Arrays.sort(points, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                return o1[1] - o2[1];
            }
        });
        int n = points.length;
        int count = 1;
        int right = points[0][1];
        for (int i = 1; i < n; i++) {
            if (points[i][0] > right) {
                count++;
                right = points[i][1];
            }
        }
        return count;
    }
```

时间复杂度：O(nlog(n))