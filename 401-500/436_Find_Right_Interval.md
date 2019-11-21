
# 436. Find Right Interval(M)

[436. 寻找右区间](https://leetcode-cn.com/problems/find-right-interval/)

## 题目描述(中等)

给定一组区间，对于每一个区间 i，检查是否存在一个区间 j，它的起始点大于或等于区间 i 的终点，这可以称为 j 在 i 的“右侧”。

对于任何区间，你需要存储的满足条件的区间 j 的最小索引，这意味着区间 j 有最小的起始点可以使其成为“右侧”区间。如果区间 j 不存在，则将区间 i 存储为 -1。最后，你需要输出一个值为存储的区间值的数组。

**注意**:
- 你可以假设区间的终点总是大于它的起始点。
- 你可以假定这些区间都不具有相同的起始点。

示例 1:
```
输入: [ [1,2] ]
输出: [-1]

解释:集合中只有一个区间，所以输出-1。
```
示例 2:
```
输入: [ [3,4], [2,3], [1,2] ]
输出: [-1, 0, 1]

解释:对于[3,4]，没有满足条件的“右侧”区间。
对于[2,3]，区间[3,4]具有最小的“右”起点;
对于[1,2]，区间[2,3]具有最小的“右”起点。
```

示例 3:
```
输入: [ [1,4], [2,3], [3,4] ]
输出: [-1, 2, -1]

解释:对于区间[1,4]和[3,4]，没有满足条件的“右侧”区间。
对于[2,3]，区间[3,4]有最小的“右”起点。
```

## 思路

- 暴力遍历
- 排序
- TreeMap
- 双数组

## 解决方法

### 暴力遍历

对每一个区间分别遍历获得其最小起始点的右区间

```java
    public int[] findRightInterval(int[][] intervals) {
        int n = intervals.length;
        int[] index = new int[n];
        for (int i = 0; i < n; i++) {
            index[i] = -1;
            for (int j = 0; j < n; j++) {
                if (intervals[j][0] >= intervals[i][1]) {
                    if (index[i] != -1) {
                        if (intervals[j][0] < intervals[index[i]][0]) {
                            index[i] = j;
                        }
                    } else {
                        index[i] = j;
                    }
                }
            }
        }
        return index;
    }

```

时间复杂度： O(n^2)
空间复杂度： O(n)

### 起点升序 + 索引表

起点升序排列，并存储每个区间的初识索引表，对每一个区间遍历到的第一个右区间即是最小起始点的右区间

```java
    public int[] findRightInterval1(int[][] intervals) {
        int n = intervals.length;
        int[] index = new int[n];
        Map<int[], Integer> map = new HashMap<>();
        for (int i = 0; i < n; i++) {
            map.put(intervals[i], i);
        }
        Arrays.sort(intervals, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                return o1[0] != o2[0] ? o1[0] - o2[0] : o1[1] - o2[1];
            }
        });
        for (int i = 0; i < n; i++) {
            index[map.get(intervals[i])] = -1;
            for (int j = i + 1; j < n; j++) {
                if (intervals[j][0] >= intervals[i][1]) {
                    index[map.get(intervals[i])] = map.get(intervals[j]);
                    break;
                }
            }
        }
        return index;
    }
```

时间复杂度： O(n^2)
空间复杂度： O(n)

### 起点升序 + 索引表 + 二分搜索

起点升序排列，并存储每个区间的初识索引表，对每一个区间去二分搜索找到右区间的左边界；

```java
    public int[] findRightInterval2(int[][] intervals) {
        int n = intervals.length;
        int[] index = new int[n];
        Map<int[], Integer> map = new HashMap<>();
        for (int i = 0; i < n; i++) {
            map.put(intervals[i], i);
        }
        Arrays.sort(intervals, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                return o1[0] != o2[0] ? o1[0] - o2[0] : o1[1] - o2[1];
            }
        });
        for (int i = 0; i < n; i++) {
            int pos = binarySearch(intervals, intervals[i][1], i + 1, n);
            index[map.get(intervals[i])] = pos != -1 ? map.get(intervals[pos]) : -1;
        }
        return index;
    }

    public int binarySearch(int[][] intervals, int target, int left, int right) {
        while (left < right) {
            int mid = (left + right) >>> 1;
            if (intervals[mid][0] < target) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        return left == intervals.length ? -1 : left;
    }
```

时间复杂度： O(nlog(n))
空间复杂度： O(n)


### TreeMap

利用TreeMap存储其区间起点和索引的关系，并根据区间起点升序排列，同上

```java
    public int[] findRightInterval3(int[][] intervals) {
        int n = intervals.length;
        int[] index = new int[n];
        TreeMap<Integer, Integer> treeMap = new TreeMap<>(new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o1 - o2;
            }
        });
        for (int i = 0; i < n; i++) {
            treeMap.put(intervals[i][0], i);
        }
        for (int i = 0; i < n; i++) {
            Integer pos = treeMap.ceilingKey(intervals[i][1]);
            index[i] = pos != null ? treeMap.get(pos) : -1;
        }
        return index;
    }
```

时间复杂度： O(nlog(n))； TreeMap 的ceilingKey查找也需要 O(log(n)) 时间
空间复杂度： O(n)

### 双数组 + 索引表

维护两个数组，
- intervals，按照起点排序
- endIntervals，按照终点排序

一旦从 endIntervals 中选择了第一个区间（或者说第 i 个区间），就可以通过在 intervals 中从左到右扫描的方法找到满足右区间要求的区间，因为 intervals 是按照起点排序的。

从intervals 中选择的元素索引为 j。那么，从 endIntervals 中选取下一个区间（第 (i+1)个区间）时，不需要从头开始扫描 intervals。相反，可以直接从上次的第 j 个开始。这是因为endIntervals[i+1] 对应的终点大于 endIntervals[i] 的，因此对于 0 < k < j 的 intervals[k] ，都不可能满足要求。



```java
    public int[] findRightInterval4(int[][] intervals) {
        int n = intervals.length;
        int[] index = new int[n];
        Map<int[], Integer> map = new HashMap<>();
        for (int i = 0; i < n; i++) {
            map.put(intervals[i], i);
        }
        int[][] endIntervals = Arrays.copyOf(intervals, n);
        Arrays.sort(intervals, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                return o1[0] - o2[0];
            }
        });
        Arrays.sort(endIntervals, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                return o1[1] - o2[1];
            }
        });
        int j = 0;
        for (int i = 0; i < n; i++) {
            while (j < n && endIntervals[i][1] > intervals[j][0]) {
                j++;
            }
            index[map.get(endIntervals[i])] = j != n ? map.get(intervals[j]) : -1;
        }
        return index;
    }
```

时间复杂度： O(nlog(n))；排序需要 O(nlog(n)) 时间；查找区间总共需要 O(n) 的时间，因为两个数组均只扫描一次。
空间复杂度： O(n)