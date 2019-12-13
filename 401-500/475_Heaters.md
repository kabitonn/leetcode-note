
# 475. Heaters(E)

[475. 供暖器](https://leetcode-cn.com/problems/heaters/)

## 题目描述(中等)

冬季已经来临。 你的任务是设计一个有固定加热半径的供暖器向所有房屋供暖。

现在，给出位于一条水平线上的房屋和供暖器的位置，找到可以覆盖所有房屋的最小加热半径。

所以，你的输入将会是房屋和供暖器的位置。你将输出供暖器的最小加热半径。

**说明**:
- 给出的房屋和供暖器的数目是非负数且不会超过 25000。
- 给出的房屋和供暖器的位置均是非负数且不会超过10^9。
- 只要房屋位于供暖器的半径内(包括在边缘上)，它就可以得到供暖。
- 所有供暖器都遵循你的半径标准，加热的半径也一样。
  
示例 1:
```
输入: [1,2,3],[2]
输出: 1
解释: 仅在位置2上有一个供暖器。如果我们将加热半径设为1，那么所有房屋就都能得到供暖。
```

示例 2:
```
输入: [1,2,3,4],[1,4]
输出: 1
解释: 在位置1, 4上有两个供暖器。我们需要将加热半径设为1，这样所有房屋就都能得到供暖。
```

## 思路

找到能给房屋供暖的最近的两旁的供暖器，计算最小距离确定该房屋有哪一个供暖；最后又最大供暖距离作为供暖半径；

- 滑动指针
- 二分搜索

## 解决方法

### 滑动指针

两个数组排序后，依次向后遍历找到大于等于房屋的供暖器索引index，即房屋右侧的供暖器，

- index == 0 :该房屋左侧无供暖器
- index == n :该房屋右侧无供暖器
- index (0,n):index 为房屋右侧， index - 1为房屋左侧

```java
    public int findRadius(int[] houses, int[] heaters) {
        int m = houses.length, n = heaters.length;
        if (m == 0 || n == 0) {
            return 0;
        }
        Arrays.sort(houses);
        Arrays.sort(heaters);
        int radius = 0;
        int index = 0;
        for (int house : houses) {
            while (index < n && house > heaters[index]) {
                index++;
            }
            int min;
            if (index == 0) {
                min = heaters[0] - house;
            } else if (index == n) {
                min = Math.abs(house - heaters[n - 1]);
            } else {
                min = Math.min(house - heaters[index - 1], heaters[index] - house);
            }
            radius = Math.max(radius, min);
        }
        return radius;
    }
```

### 二分搜索

将供暖器位置排序，利用二分搜索找到第一个大于等于房屋的索引left；
- left == n :该房屋右侧无供暖器
- left == 0 :该房屋左侧无供暖器
- left (0,n):left为房屋右侧，left-1为房屋左侧

```java
    public int findRadius1(int[] houses, int[] heaters) {
        int m = houses.length, n = heaters.length;
        if (m == 0 || n == 0) {
            return 0;
        }
        Arrays.sort(heaters);
        int radius = 0;
        for (int house : houses) {
            int left = 0, right = n;
            while (left < right) {
                int mid = (left + right) >>> 1;
                if (heaters[mid] < house) {
                    left = mid + 1;
                } else {
                    right = mid;
                }
            }
            int min;
            if (left == n) {
                min = house - heaters[n - 1];
            } else if (left == 0) {
                min = heaters[0] - house;
            } else {
                min = Math.min(heaters[left] - house, house - heaters[left - 1]);
            }
            radius = Math.max(radius, min);
        }
        return radius;
    }
```