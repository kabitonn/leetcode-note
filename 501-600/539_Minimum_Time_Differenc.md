
# 539. Minimum Time Differenc(M)

[539. 最小时间差](https://leetcode-cn.com/problems/minimum-time-difference/)

## 题目描述(中等)

给定一个 24 小时制（小时:分钟）的时间列表，找出列表中任意两个时间的最小时间差并已分钟数表示。


示例 1：
```
输入: ["23:59","00:00"]
输出: 1
```

**备注**:

- 列表中时间数在 2~20000 之间。
- 每个时间取值在 00:00~23:59 之间。


## 思路

## 解决方法

### 排序 比较差值

- 时间转化为分钟数
- 然后对数字进行排序，进行比较
- 头部和尾部时间的比较时考虑不同的方向

```java
    public int findMinDifference(List<String> timePoints) {
        int n = timePoints.size();
        if (n < 2) {
            return 0;
        }
        int[] times = new int[n];
        for (int i = 0; i < n; i++) {
            //String[] strs = timePoints.get(i).split(":");
            //times[i] = Integer.valueOf(strs[0]) * 60 + Integer.valueOf(strs[1]);
            String time = timePoints.get(i);
            times[i] = Integer.valueOf(time.substring(0, 2)) * 60 + Integer.valueOf(time.substring(3));
        }
        Arrays.sort(times);
        int min = times[0] + 24 * 60 - times[n - 1];
        for (int i = 1; i < n; i++) {
            min = Math.min(times[i] - times[i - 1], min);
        }
        return min;
    }
```

### 桶排序


```java
    public int findMinDifference2(List<String> timePoints) {
        int n = timePoints.size();
        if (n < 2) {
            return 0;
        }
        if (n > 24 * 60) {
            return 0;
        }
        int[] map = new int[24 * 60];
        for (String time : timePoints) {
            int t = Integer.valueOf(time.substring(0, 2)) * 60 + Integer.valueOf(time.substring(3));
            if (map[t] != 0) {
                return 0;
            }
            map[t] = 1;
        }
        int first = -1, prev = -1;
        int min = 24 * 60;
        for (int i = 0; i < map.length; i++) {
            if (map[i] == 0) {
                continue;
            }
            if (prev == -1) {
                first = i;
            } else {
                min = Math.min(min, i - prev);
            }
            prev = i;
        }
        min = Math.min(first + 24 * 60 - prev, min);
        return min;
    }
```