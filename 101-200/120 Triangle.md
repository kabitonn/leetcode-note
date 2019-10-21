# 120. Triangle(M)


[120. 三角形最小路径和](https://leetcode-cn.com/problems/triangle/)


## 题目描述(中等)
Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

For example, given the following triangle
```
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
The minimum path sum from top to bottom is 11 (i.e., 2 + 3 + 5 + 1 = 11).
```
**Note**:

Bonus point if you are able to do this using only O(n) extra space, where n is the total number of rows in the triangle.



## 思路

- DFS 递归
- 动态规划


## 解决方法


### DFS 递归

数里边调用了两次自己，所以导致进行了很多重复的搜索，所以肯定会导致超时

```java
    public int minimumTotal(List<List<Integer>> triangle) {
        if (triangle.size() == 0 || triangle.get(0).size() == 0) {
            return 0;
        }
        return minimumTotal(triangle, 0, 0, 0);
    }

    public int minimumTotal(List<List<Integer>> triangle, int row, int index, int sum) {
        sum += triangle.get(row).get(index);
        if (row == triangle.size() - 1) {
            return sum;
        }
        int sum0 = minimumTotal(triangle, row + 1, index, sum);
        int sum1 = minimumTotal(triangle, row + 1, index + 1, sum);
        return Math.min(sum0, sum1);
    }
```


### 递归 记忆化

```java
    public int minimumTotal0(List<List<Integer>> triangle) {
        int size = triangle.size();
        if (size == 0 || triangle.get(0).size() == 0) {
            return 0;
        }
        Integer[][] map = new Integer[size][];
        for (int i = size - 1; i >= 0; i--) {
            map[i] = new Integer[i + 1];
        }
        return minimumTotal(triangle, 0, 0, map);
    }

    public int minimumTotal(List<List<Integer>> triangle, int row, int index, Integer[][] map) {
        if (row == triangle.size()) {
            return 0;
        }
        if (map[row][index] != null) {
            return map[row][index];
        }
        int sum0 = minimumTotal(triangle, row + 1, index, map);
        int sum1 = minimumTotal(triangle, row + 1, index + 1, map);
        int sum = Math.min(sum0, sum1) + triangle.get(row).get(index);
        map[row][index] = sum;
        return sum;
    }
```

### 动态规划


```java
    public int minimumTotal1(List<List<Integer>> triangle) {
        int size = triangle.size();
        if (size == 0 || triangle.get(0).size() == 0) {
            return 0;
        }
        int[][] pathSum = new int[size + 1][];
        for (int i = size; i >= 0; i--) {
            pathSum[i] = new int[i + 1];
            if (i == size) {
                continue;
            }
            List<Integer> row = triangle.get(i);
            for (int j = 0; j <= i; j++) {
                pathSum[i][j] = Math.min(pathSum[i + 1][j], pathSum[i + 1][j + 1]) + row.get(j);
            }
        }
        return pathSum[0][0];
    }

```

### 动态规划空间优化

```java
    public int minimumTotal2(List<List<Integer>> triangle) {
        int size = triangle.size();
        if (size == 0 || triangle.get(0).size() == 0) {
            return 0;
        }
        int[] pathSum = new int[size + 1];
        for (int i = size - 1; i >= 0; i--) {
            List<Integer> row = triangle.get(i);
            for (int j = 0; j <= i; j++) {
                pathSum[j] = Math.min(pathSum[j], pathSum[j + 1]) + row.get(j);
            }
        }
        return pathSum[0];
    }
```



