
# 497. Random Point in Non-overlapping Rectangles(M)

[497. 非重叠矩形中的随机点](https://leetcode-cn.com/problems/random-point-in-non-overlapping-rectangles/)

## 题目描述(中等)

给定一个非重叠轴对齐矩形的列表 rects，写一个函数 pick 随机均匀地选取矩形覆盖的空间中的整数点。

提示：
- 整数点是具有整数坐标的点。
- 矩形周边上的点包含在矩形覆盖的空间中。
- 第 i 个矩形 `rects [i] = [x1，y1，x2，y2]`，其中 `[x1，y1]` 是左下角的整数坐标，`[x2，y2]` 是右上角的整数坐标。
- 每个矩形的长度和宽度不超过 2000。
- `1 <= rects.length <= 100`
- pick 以整数坐标数组 `[p_x, p_y]` 的形式返回一个点。
- pick 最多被调用10000次。
 

示例 1：
```
输入: 
["Solution","pick","pick","pick"]
[[[[1,1,5,5]]],[],[],[]]
输出: 
[null,[4,1],[4,1],[3,3]]
```

示例 2：

```
输入: 
["Solution","pick","pick","pick","pick","pick"]
[[[[-2,-2,-1,-1],[1,0,3,0]]],[],[],[],[],[]]
输出: 
[null,[-1,-2],[2,0],[-2,-1],[3,0],[-2,-2]]
```

**输入语法的说明**：

输入是两个列表：调用的子例程及其参数。 `Solution` 的构造函数有一个参数，即矩形数组 `rects` 。 `pick` 没有参数。参数总是用列表包装的，即使没有也是如此。


## 思路

随机均匀选取依据的权重是矩形内整数点的个数，所以对矩形内包含的整数点个数而不是面积进行统计，随机生成一个[0,sum)之间的数字，判断这个数字属于哪个矩形内部；当前矩形有num个整数点，之前有s个整数点，若生成的随机数在[s,s+num)之间，表示属于这个举行内部的点

- 利用TreeMap排序
- 利用二分查找

## 解决方法

### TreeMap

TreeMap 存储当前第i个矩形前的整数点的个数，随机生成一个数字r,找到小于等于r的矩形索引，在该矩形内部获取一个整数点
- 在该矩形内部随机生成一个整数点
- 利用之前生成的随机数判断是该矩形内部的哪个整数点

在该矩形内部随机生成一个整数点，横纵坐标随机生成

```java
public class S497RandomPointInNonOverlappingRectangles {

    int[][] rects;
    Random random;
    TreeMap<Integer, Integer> area;
    int sumArea;

    public S497RandomPointInNonOverlappingRectangles(int[][] rects) {
        this.rects = rects;
        this.random = new Random();
        area = new TreeMap<>();
        sumArea = 0;
        int i = 0;
        for (int[] rect : rects) {
            area.put(sumArea, i++);
            sumArea += (rect[2] - rect[0] + 1) * (rect[3] - rect[1] + 1);
        }
    }

    public int[] pick() {
        int r = random.nextInt(sumArea);
        int pos = area.get(area.floorKey(r));
        int[] rect = rects[pos];
        int px = rect[0] + random.nextInt(rect[2] - rect[0] + 1);
        int py = rect[1] + random.nextInt(rect[3] - rect[1] + 1);
        return new int[]{px, py};
    }
}
```

利用之前生成的随机数判断是该矩形内部的哪个整数点，首先确定该随机数是该矩形内第几个整数点，依据一定规律判断是那个点

![](../assets/leetcode-note/401-500/497-s-1-2.png ':size=300')

```java
    public int[] pick1() {
        int target = random.nextInt(sumArea);
        int index = area.get(area.floorKey(target));
        int[] rect = rects[index];
        int pos = target - area.floorKey(target);
        int px = rect[0] + pos % (rect[2] - rect[0] + 1);
        int py = rect[1] + pos / (rect[2] - rect[0] + 1);
        return new int[]{px, py};
    }
```

### 二分查找

areaSum[i] 存储第i个矩形前有多少个整数点(包括第i个)，随机生成一个数字target，利用二分查找找到第一个大于target的索引，即该索引之前的矩形内的整数点都不在范围内

在该矩形内部获取一个整数点
- 在该矩形内部随机生成一个整数点
- 利用之前生成的随机数判断是该矩形内部的哪个整数点，首先确定之前的矩形内有多少个整数点，即判断当前矩形是不是第一个


```java
public class S497RandomPointInNonOverlappingRectangles_1 {

    int[][] rects;
    Random random;
    int[] areaSum;

    public S497RandomPointInNonOverlappingRectangles_1(int[][] rects) {
        this.rects = rects;
        this.random = new Random();
        this.areaSum = new int[rects.length];
        int sum = 0;
        int i = 0;
        for (int[] rect : rects) {
            sum += (rect[2] - rect[0] + 1) * (rect[3] - rect[1] + 1);
            areaSum[i++] = sum;
        }
    }

    public int[] pick() {
        int target = random.nextInt(areaSum[areaSum.length - 1]);
        int left = 0, right = areaSum.length - 1;
        while (left < right) {
            int mid = (left + right) >>> 1;
            if (areaSum[mid] <= target) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        int[] rect = rects[left];
        int px = rect[0] + random.nextInt(rect[2] - rect[0] + 1);
        int py = rect[1] + random.nextInt(rect[3] - rect[1] + 1);
        return new int[]{px, py};
    }

    public int[] pick1() {
        int target = random.nextInt(areaSum[areaSum.length - 1]);
        int left = 0, right = areaSum.length - 1;
        while (left < right) {
            int mid = (left + right) >>> 1;
            if (areaSum[mid] <= target) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        int pos = left != 0 ? target - areaSum[left - 1] : target;
        int[] rect = rects[left];
        int px = rect[0] + pos % (rect[2] - rect[0] + 1);
        int py = rect[1] + pos / (rect[2] - rect[0] + 1);
        return new int[]{px, py};
    }
}
```