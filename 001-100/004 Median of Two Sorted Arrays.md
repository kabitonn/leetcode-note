# 004. Median of Two Sorted Arrays(H)
[004. Median of Two Sorted Arrays](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)


## 题目描述\(困难\)

There are two sorted arrays nums1 and nums2 of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log(m+n)).

You may assume nums1 and nums2 cannot be both empty.

Example 1:

```
nums1 = [1, 3]
nums2 = [2]
The median is 2.0
```

Example 2:

```
nums1 = [1, 2]
nums2 = [3, 4]
The median is (2 + 3)/2 = 2.5
```

## 思路

1. 归并全部数组
2. 归并部分数组，知道中位数出现
3. 利用中位数定义，将两个数组划分为数量相等的两部分

## 解决方法

### 暴力法-归并数组

归并两个有序数组，返回中位数

```java
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m=nums1.length;
        int n=nums2.length;
        int i=0,j=0;
        int mid = (m+n)/2;
        int[] nums = new int[m+n];
        int k=0;

        boolean isEven = (m+n)%2==0?true:false;
        while((k<=mid)&&(i<m||j<n)) {
            while(i<m&&(j>=n||nums1[i]<nums2[j])) {
                nums[k++] = nums1[i++];
            }
            while(j<n&&(i>=m||nums1[i]>=nums2[j])) {
                nums[k++] = nums2[j++];
            }
        }
        if(isEven) {
            return (nums[mid-1]+nums[mid])/2.0;
        }
        else {
            return nums[mid];
        }
    }
```

### 暴力法-归并部分数组

不需要归并全部数组，根据奇偶数只需归并中间部分的两个数；

用 len 表示合并后数组的长度，如果是奇数，我们需要知道第 (len + 1)/ 2 个数就可以了，如果遍历的话需要遍历  len / 2 + 1次。如果是偶数，我们需要知道第 len / 2 和 len / 2 + 1 个数，也是需要遍历 len / 2 + 1次。所以遍历的话，奇数和偶数都是 len / 2 + 1 次。

```java
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m=nums1.length;
        int n=nums2.length;
        int pre=0,cur=0;
        int i=0,j=0;

        int mid = (m+n)/2;
        boolean isEven = (m+n)%2==0?true:false;
        for(int k=0;k<=mid;k++) {
            pre = cur;
            if(i<m&&(j>=n||nums1[i]<nums2[j])) {
                cur = nums1[i++];
            }
            else if(j<n&&(i>=m)||nums1[i]>=nums2[j]) {
                cur = nums2[j++];
            }
        }
        if(isEven) {
            return (pre+cur)/2.0;
        }
        else {
            return cur;
        }

    }
```

时间复杂度：遍历  len / 2 + 1 次， len = m + n ，所以时间复杂度依旧是  O(m + n) 。

空间复杂度：我们申请了常数个变量，所以空间复杂度是  O(1) 。

### 递归（迭代）

> 中位数（又称中值，英语：Median），统计学中的专有名词，代表一个样本、种群或概率分布中的一个数值，其可将数值集合划分为相等的上下两部分。
>
> 将一个集合划分为两个长度相等的子集，其中一个子集中的元素总是大于另一个子集中的元素  
> 首先，让我们在任一位置  i  将  A 划分成两个部分：

```
      left_A             |        right_A
A[0], A[1], ..., A[i-1]  |  A[i], A[i+1], ..., A[m-1]
```

由于 A  中有 m 个元素， 所以我们有 m+1 种划分的方法 (i=0...m) 

> len\(leftA\) = i, len\(rightA\) = m - i  
> 当 i = 0 时，leftA为空集，当 i = m 时，rightA为空集

采用同样的方式，我们在任一位置 j 将 B 划分成两个部分：

```
      left_B             |        right_B
B[0], B[1], ..., B[j-1]  |  B[j], B[j+1], ..., B[n-1]
```

将

```
      left_part          |        right_part
A[0], A[1], ..., A[i-1]  |  A[i], A[i+1], ..., A[m-1]
B[0], B[1], ..., B[j-1]  |  B[j], B[j+1], ..., B[n-1]
```

![](/assets/001-100/004-solution-3-1.png)

如果我们可以确认：  
    1.  len(leftpart) = len(rightpart)   
    2.  max(leftpart) < = min(rightpart)   
那么，我们已经将A,B两个数组所有元素划分为相同长度的两个部分，且其中一部分中的元素总是大于另一部分中的元素。那么：   median = (max(leftpart)+min(rightpart)) / 2

* 当 A 数组和 B 数组的总长度是偶数时，如果我们能够保证
  * 左半部分的长度等于右半部分
    * i + j = m - i + n - j , 也就是 j = ( m + n ) / 2 - i
  * 左半部分最大的值小于等于右半部分最小的值  max ( A [ i - 1 ] , B [ j - 1 ]) < = min ( A [ i ] , B [ j ])
    * 那么，中位数就可以表示（左半部分最大值 + 右半部分最小值 ）/ 2 （max  ( A [ i - 1 ] , B [ j - 1 ]）+ min ( A [ i ] , B [ j ]）） / 2
* 当 A 数组和 B 数组的总长度是奇数时，如果我们能够保证
  * 左半部分的长度比右半部分大 1
    * i + j = m - i + n - j + 1 也就是 j = ( m + n + 1) / 2 - i 
  * 左半部分最大的值小于等于右半部分最小的值 max ( A [ i - 1 ] , B [ j - 1 ]) < = min ( A [ i ] , B [ j ]) 
    * 那么，中位数就是左半部分最大值，也就是左半部比右半部分多出的那一个数。 max ( A [ i - 1 ] , B [ j - 1 ])

上边的第一个条件我们其实可以合并为 j = ( m + n + 1) / 2 - i，因为如果 m + n 是偶数，由于我们取的是 int 值，所以加 1 也不会影响结果。当然，由于 0 < = i < = m ，为了保证 0 < = j < = n  ，我们必须保证  m < = n 。

* $$ m\leq n,i < m,j=(m+n+1)/2-i \geq (m+m+1)/2-i > (m+m+1)/2-m=0 $$
* $$ m\leq n,i > 0,j=(m+n+1)/2-i \leq (n+n+1)/2-i < (n+n+1)/2=n $$

对于第二个条件，奇数和偶数的情况是一样的，我们进一步分析。为了保证  max ( A [ i - 1 ] , B [ j - 1 ]) < = min ( A [ i ] , B [ j ]) ，因为 A 数组和 B 数组是有序的，所以 A [ i - 1 ] < = A [ i ],B [ i - 1 ] < = B [ i ] 这是天然的，所以我们只需要保证 B [ j - 1 ] < = A [ i ] 和 A [ i - 1 ] < = B [ j ] 所以我们分两种情况讨论：

* B [ j - 1 ] > A [ i ]，并且为了不越界，要保证  j != 0，i != m 
  * 此时很明显，我们需要增加 i ，为了数量的平衡还要减少 j ，幸运的是 j = ( m + n + 1) / 2 - i，i 增大，j 自然会减少。
* A [ i - 1 ] > B [ j ]，并且为了不越界，要保证  i != 0，j != n 
  * 此时和上边的情况相反，我们要减少 i ，增大 j 。

此外，考虑边界情况

* 当 i = 0 或者 j = 0 ，也就是切在了最前边。
  * 此时左半部分当 j = 0 时，最大的值就是 A [ i - 1 ] ；当 i = 0 时 最大的值就是 B [ j - 1] 。右半部分最小值和之前一样。
* 当 i = m 或者 j = n ，也就是切在了最后边。
  * 此时左半部分最大值和之前一样。右半部分当 j = n 时，最小值就是  A [ i ] ；当 i = m 时，最小值就是B [ j ] 。左半部分最大值和之前一样。

增加 i 的方式。采用二分。初始化 i 为中间的值，然后减半找中间的，减半找中间的，减半找中间的直到答案。

```java
    public double findMedianSortedArrays(int[] A, int[] B) {
        int m = A.length;
        int n = B.length;
        if (m > n) { 
            return findMedianSortedArrays(B,A); // 保证 m <= n
        }
        int iMin = 0, iMax = m;
        while (iMin <= iMax) {
            int i = (iMin + iMax) / 2;
            int j = (m + n + 1) / 2 - i;
            if (j != 0 && i != m && B[j-1] > A[i]){ // i 需要增大
                iMin = i + 1; 
            }
            else if (i != 0 && j != n && A[i-1] > B[j]) { // i 需要减小
                iMax = i - 1; 
            }
            else { // 达到要求，并且将边界条件列出来单独考虑
                int maxLeft = 0; 
                if (i == 0) { maxLeft = B[j-1]; }
                else if (j == 0) { maxLeft = A[i-1]; }
                else { maxLeft = Math.max(A[i-1], B[j-1]); }
                if ( (m + n) % 2 == 1 ) { return maxLeft; } // 奇数的话不需要考虑右半部分

                int minRight = 0;
                if (i == m) { minRight = B[j]; }
                else if (j == n) { minRight = A[i]; }
                else { minRight = Math.min(B[j], A[i]); }

                return (maxLeft + minRight) / 2.0; //如果是偶数的话返回结果
            }
        }
        return 0.0;
    }
```

时间复杂度：我们对较短的数组进行了二分查找，所以时间复杂度是  O(log(min(m，n)))。

空间复杂度：只有一些固定的变量，和数组长度无关，所以空间复杂度是 O ( 1 ) 。

