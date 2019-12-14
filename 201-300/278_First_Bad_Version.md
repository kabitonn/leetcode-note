# 278. First Bad Version(E)
[278. First Bad Version](https://leetcode-cn.com/problems/first-bad-version/)

## 题目描述\(简单\)

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have n versions \[1, 2, ..., n\] and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API bool isBadVersion\(version\) which will return whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

Example:

```
Given n = 5, and version = 4 is the first bad version.

call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true

Then 4 is the first bad version.
```

## 思路

二分法求true的左边界

## 解决方法

### 二分法左边界

```java
    public int firstBadVersion(int n) {
        int left = 1;
        int right = n;
        while(left<right) {
            int mid = left+(right-left)/2;
            if(isBadVersion(mid)) {
                right = mid;
            }
            else {
                left = mid+1;
            }
        }
        return left;
    }
```
时间复杂度：$O(\log n)$。搜索空间每次减少一半，因此时间复杂度为 $O(\log n)$。
空间复杂度：$O(1)$。

### 线性查找(超时)


```java
    public int firstBadVersion(int n) {
        for (int i = 1; i < n; i++) {
            if (isBadVersion(i)) {
                return i;
            }
        }
        return n;
    }
```
时间复杂度：$O(n)$。在最坏的情况下，最多可能会调用 isBadVersion $n-1$ 次，因此总的时间复杂度为 $O(n)$。
空间复杂度：$O(1)$。





