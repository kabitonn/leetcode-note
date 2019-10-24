# 274. H-Index\(M\)



## 题目描述\(中等\)

Given an array of citations \(each citation is a non-negative integer\) of a researcher, write a function to compute the researcher's h-index.

According to the definition of h-index on Wikipedia: "A scientist has index h if h of his/her N papers have **at least** h citations each, and the other N − h papers have **no more than** h citations each."

h 指数的定义: “h 代表“高引用次数”（high citations），一名科研人员的 h 指数是指他（她）的 （N 篇论文中）至多有 h 篇论文分别被引用了至少 h 次。（其余的 N - h 篇论文每篇被引用次数不多于 h 次。）”

Example:

```
Input: citations = [3,0,6,1,5]
Output: 3 
Explanation: [3,0,6,1,5] means the researcher has 5 papers in total and each of them had 
             received 3, 0, 6, 1, 5 citations respectively. 
             Since the researcher has 3 papers with at least 3 citations each and the remaining 
             two with no more than 3 citations each, her h-index is 3.
```

**Note**: If there are several possible values for h, the maximum one is taken as the h-index.

## 思路

* 排序
* 计数

## 解决方法

### 排序

![](/assets/201-300/274-s-1-1.png)



```java
    public int hIndex(int[] citations) {
        Arrays.sort(citations);
        int len = citations.length;
        int h = 0;
        // 至多有 h 篇论文分别被引用了至少 h 次
        while (h < len && citations[len - h - 1] >= h + 1) {
            h++;
        }
        return h;
    }
```

```java
    public int hIndex0(int[] citations) {
        Arrays.sort(citations);
        int len = citations.length;
        int index = len - 1;
        while (index >= 0 && citations[index] >= len - index) {
            index--;
        }
        return len - index - 1;
    }
```

### 排序 二分搜索

```java
    public int hIndex2(int[] citations) {
        Arrays.sort(citations);
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
        return len - left;
    }
```

### 计数



![](/assets/201-300/274-s-3-1.png)



```java
    public int hIndex1(int[] citations) {
        int n = citations.length;
        int[] papers = new int[n + 1];
        // 计数
        for (int c : citations) {
            papers[Math.min(n, c)]++;
        }
        // 找出最大的 k
        int k = n;
        for (int s = papers[n]; k > s; s += papers[k]) {
            k--;
        }
        return k;
    }
```



