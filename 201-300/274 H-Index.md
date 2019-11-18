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

首先我们将引用次数降序排序，在排完序的数组 citations 中，如果 citations\[i\]&gt;i，那么说明第 0 到 i 篇论文都有至少 i+1 次引用。因此我们只要找到最大的 i 满足 citations\[i\]&gt;i，那么 h 指数即为 i+1。例如：

```java
    public int hIndex(int[] citations) {
        Arrays.sort(citations);
        int len = citations.length;
        int h = 0;
        // 倒序扫描
        // citations[i] > i
        while (h < len && citations[len - h - 1] > h) {
            h++;
        }
        return h;
    }

    public int hIndex0_1(int[] citations) {
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

h = len - index - 1

跳出循环时,index + 1为最后满足citations\[index\] &gt;= len - index的索引，  
至多有len-\(index + 1\)篇被引用了至少h篇\(h=len-index-1\)

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

时间复杂度：O\(nlogn\)，即为排序的时间复杂度。

### 排序 二分搜索

找到满足citations\[i\] &gt;= len - i 的左边界的索引 i ，

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
        //left为满足citations[i] >= len - i 的左边界的索引
        if (left < len && citations[left] < len - left) {
            left++;
        }
        return len - left;
    }

    public int hIndex2_1(int[] citations) {
        Arrays.sort(citations);
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
        //left为满足citations[i] >= len - i 的左边界的索引
        return len - left;
    }
```

找到满足citations\[i\] &gt;= len - i 的左边界的 i ，

### 计数

论文的引用次数可能会非常多，这个数值很可能会超过论文的总数 nn，因此使用计数排序是非常不合算的（会超出空间限制）。在这道题中，我们可以通过一个不难发现的结论来让计数排序变得有用，

> 如果一篇文章的引用次数超过论文的总数 nn，那么将它的引用次数降低为 nn 也不会改变 hh 指数的值。

由于 h 指数一定小于等于 n，因此这样做是正确的。在直方图中，将所有超过 y 轴值大于 n 的变为 n 等价于去掉 y&gt;n 的整个区域。

![](/assets/201-300/274-s-3-1.png)



我们用一个例子来说明如何使用计数排序得到 h 指数。首先，引用次数如下所示：

$$ citations=[1,3,2,3,100] $$

将所有大于 n=5 的引用次数变为 n，得到：

$$ citations=[1,3,2,3,5] $$

计数排序得到的结果如下：

| k | 0 | 1 | 2 | 3 | 4 | 5 |
| --- | --- | --- | --- | --- | --- | --- |
| count | 0 | 1 | 1 | 2 | 0 | 1 |
| sk | 5 | 5 | 4 | 3 | 1 | 1 |

其中 sk表示至少有 k 次引用的论文数量，在表中即为在它之后的列（包括本身）的 count 一行的和。根据定义，最大的满足 k&lt;=sk 的 k 即为所求的 h。在表中，这个 k 为 3，因此 h 指数为 3。

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

时间复杂度：O\(n\)。在计数时，我们仅需要遍历 citations 数组一次，因此时间复杂度为 O\(n\)。在找出最大的 k 时，我们最多需要遍历计数的数组一次，而计数的数组的长度为 O\(n\)，因此这一步的时间复杂度为 O\(n\)，即总的时间复杂度为 O\(n\)。  
空间复杂度：O\(n\)。我们需要使用 O\(n\) 的空间来存放计数的结果。

