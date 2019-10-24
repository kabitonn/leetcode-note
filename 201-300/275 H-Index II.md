# 275. H-Index II(M)

[275. H指数 II](https://leetcode-cn.com/problems/h-index-ii/)

## 题目描述(中等)

Given an array of citations **sorted in ascending order** (each citation is a non-negative integer) of a researcher, write a function to compute the researcher's h-index.

According to the definition of h-index on Wikipedia: "A scientist has index h if h of his/her N papers have **at least** h citations each, and the other N − h papers have **no more than** h citations each."

h 指数的定义: “h 代表“高引用次数”（high citations），一名科研人员的 h 指数是指他（她）的 （N 篇论文中）**至多**有 h 篇论文分别被引用了**至少** h 次。（其余的 N - h 篇论文每篇被引用次数**不多于** h 次。）"


Example:
```
Input: citations = [0,1,3,5,6]
Output: 3 
Explanation: [0,1,3,5,6] means the researcher has 5 papers in total and each of them had 
             received 0, 1, 3, 5, 6 citations respectively. 
             Since the researcher has 3 papers with at least 3 citations each and the remaining 
             two with no more than 3 citations each, her h-index is 3.
```
**Note**:

If there are several possible values for h, the maximum one is taken as the h-index.

**Follow up**:

- This is a follow up problem to H-Index, where citations is now guaranteed to be sorted in ascending order.
- Could you solve it in logarithmic time complexity?


## 思路

- 二分搜素

## 解决方法

### 二分搜索

当 i 满足至多有h篇论文至少被引用h次时，h = len - i
即满足 citations[i] >= len - i 
找到满足该条件的左边界

下面两种方法

搜索区间为[0,len)
```java
    public int hIndex(int[] citations) {
        int len = citations.length;
        int left = 0;
        int right = len;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (citations[mid] < len - mid) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        return len - left;
    }
```

搜索区间为[0,len-1)
最后判断left是否为满足条件的左边界，否则 +1
```
    public int hIndex2(int[] citations) {
        int len = citations.length;
        int left = 0;
        int right = len - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (citations[mid] < len - mid) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        //left为满足citations[i] >= len - i 的左边界的索引
        if (left < len && citations[left] < len - left) {
            left++;
        }
        return len - left;
    }

```