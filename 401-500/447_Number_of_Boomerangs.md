
# 447. Number of Boomerangs(E)

[447. 回旋镖的数量](https://leetcode-cn.com/problems/number-of-boomerangs/)

## 题目描述(中等)

给定平面上 n 对不同的点，“回旋镖” 是由点表示的元组 `(i, j, k)` ，其中 i 和 j 之间的距离和 i 和 k 之间的距离相等（需要考虑元组的顺序）。

找到所有回旋镖的数量。你可以假设 n 最大为 500，所有点的坐标在闭区间 [-10000, 10000] 中。

示例:
```
输入:
[[0,0],[1,0],[2,0]]

输出:
2

解释:
两个回旋镖为 [[1,0],[0,0],[2,0]] 和 [[1,0],[2,0],[0,0]]
```


## 思路

- 暴力遍历

## 解决方法

### 暴力遍历

遍历所有的`(i,j,k)`元组，判断是否可以构成回旋镖

```java
    public int numberOfBoomerangs(int[][] points) {
        int num = 0;
        for (int i = 0; i < points.length; i++) {
            for (int j = 0; j < points.length; j++) {
                for (int k = 0; k < points.length; k++) {
                    if (i == j || i == k || j == k) {
                        continue;
                    }
                    if (getDistance(points[i], points[j]) == getDistance(points[i], points[k])) {
                        num += 1;
                    }
                }
            }
        }
        return num;
    }

    public int getDistance(int[] point1, int[] point2) {
        int distance = 0;
        for (int i = 0; i < point1.length; i++) {
            distance += (point1[i] - point2[i]) * (point1[i] - point2[i]);
        }
        return distance;
    }
```

时间复杂度： O(n^3)

### 遍历优化

遍历有序的元组(i,j,k),三个点之间能否构成回旋镖，若能，即可构成两种，否则为0  

```java
    public int numberOfBoomerangs0(int[][] points) {
        int num = 0;
        for (int i = 0; i < points.length - 2; i++) {
            for (int j = i + 1; j < points.length - 1; j++) {
                for (int k = j + 1; k < points.length; k++) {
                    num += isEqualDistance(points[i], points[j], points[k]);
                }
            }
        }
        return num;
    }


    public int isEqualDistance(int[] point1, int[] point2, int[] point3) {
        int distance1 = getDistance(point1,point2);
        int distance2 = getDistance(point1,point3);
        int distance3 = getDistance(point2,point3);
        if (distance1==distance2||distance1==distance3||distance2==distance3) {
            return 2;
        }
        return 0;
    }

```
时间复杂度： O(n^3)


### 哈希表计数

遍历每一个点作为回旋镖中心，计算其余点和它的距离，若有重复，即可构成回旋镖
- 若重复数目为n,可构成不同的 n * (n - 1)种

```java 
    public int numberOfBoomerangs1(int[][] points) {
        int num = 0;
        Map<Integer,Integer> map = new HashMap<>();
        for (int i = 0; i < points.length; i++) {
            for(int j=0;j<points.length;j++){
                if(i==j){continue;}
                int distance = getDistance(points[i],points[j]);
                map.put(distance, map.getOrDefault(distance, 0)+1);
            }
            for(int key:map.keySet()){
                if(map.get(key)!=1){
                    int n = map.get(key);
                    num+=(n)*(n-1);
                }
            }
            map.clear();
        }
        return num;
    }
```

时间复杂度： O(n^2)


### 哈希表计数

遍历每一个点作为回旋镖中心，计算其余点和它的距离，此时重复距离的数目为 n, 可构造新的回旋镖数量为 (n-1)*2;

```java
    public int numberOfBoomerangs2(int[][] points) {
        int num = 0;
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < points.length; i++) {
            for (int j = 0; j < points.length; j++) {
                if (i == j) {
                    continue;
                }
                int distance = getDistance(points[i], points[j]);
                map.put(distance, map.getOrDefault(distance, 0) + 1);
                num += (map.get(distance) - 1) * 2;
            }
            map.clear();
        }
        return num;
    }
```

时间复杂度： O(n^2)
