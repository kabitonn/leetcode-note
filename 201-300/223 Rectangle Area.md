# 223. Rectangle Area\(M\)

[223. 矩形面积](https://leetcode-cn.com/problems/rectangle-area/)

## 题目描述\(中等\)

Find the total area covered by two rectilinear rectangles in a 2D plane.

Each rectangle is defined by its bottom left corner and top right corner as shown in the figure.

![](/assets/201-300/223-p-1.png)

Example:

```
Input: A = -3, B = 0, C = 3, D = 4, E = 0, F = -1, G = 9, H = 2
Output: 45
```

## 思路

## 解决方法

### 

```java
    public int computeArea1(int A, int B, int C, int D, int E, int F, int G, int H) {
        int areaSum = (C - A) * (D - B) + (G - E) * (H - F);
        if (C < E || A > G) {
            return areaSum;
        }
        if (D < F || B > H) {
            return areaSum;
        }
        int left = Math.max(A, E);
        int right = Math.min(C, G);
        int up = Math.min(D, H);
        int down = Math.max(B, F);
        return areaSum - (right - left) * (up - down);
    }
```



