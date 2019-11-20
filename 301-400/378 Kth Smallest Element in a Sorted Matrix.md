# 378. Kth Smallest Element in a Sorted Matrix(M)

[378. 有序矩阵中第K小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix/)

## 题目描述(中等)

给定一个 `n x n` 矩阵，其中每行和每列元素均按升序排序，找到矩阵中第k小的元素。
请注意，它是排序后的第k小元素，而不是第k个元素。

示例:
```
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

返回 13。
```
**说明**:
你可以假设 k 的值永远是有效的, 1 ≤ k ≤ n^2 。


## 思路

- 全排序
- 堆排序
- 二分搜索

## 解决方法

### 全排序

```java
   public int kthSmallest(int[][] matrix, int k) {
        int n = matrix.length;
        int[] nums = new int[n * n];
        int index = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                nums[index++] = matrix[i][j];
            }
        }
        Arrays.parallelSort(nums);
        return nums[k - 1];
    }

```

### 堆 优先队列

大根堆留下k个最小的，队首即为第k小；

小根堆弹出k-1次，队首即为第k小

```java
   public int kthSmallest1_1(int[][] matrix, int k) {
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>(k + 1, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1;
            }
        });
        int n = matrix.length;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                maxHeap.add(matrix[i][j]);
                if (maxHeap.size() > k) {
                    maxHeap.poll();
                }
            }
        }
        return maxHeap.peek();
    }

    public int kthSmallest1_2(int[][] matrix, int k) {
        int n = matrix.length;
        PriorityQueue<Integer> minHeap = new PriorityQueue<>(n * n);
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                minHeap.add(matrix[i][j]);
            }
        }
        for (int i = 0; i < k - 1; i++) {
            minHeap.poll();
        }
        return minHeap.peek();
    }

```

### 二分搜索

1. 找出二维矩阵中最小的数left，最大的数right，那么第k小的数必定在left~right之间
2. mid=(left+right) / 2；在二维矩阵中寻找小于等于mid的元素个数count
3. 若这个count小于k，表明第k小的数在右半部分且不包含mid，即left=mid+1, 又保证了第k小的数在left~right之间
4. 若这个count大于k，表明第k小的数在左半部分且可能包含mid，即right=mid，又保证了第k小的数在left~right之间
5. 因为每次循环中都保证了第k小的数在left~right之间，当left==right时，第k小的数即被找出，等于left



```java
   public int kthSmallest2(int[][] matrix, int k) {
        int n = matrix.length;
        int left = matrix[0][0], right = matrix[n - 1][n - 1];
        while (left < right) {
            int mid = (left + right) >>> 1;
            int count = countLeqNumber(matrix, n, mid);
            if (count < k) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        return left;
    }

    //求count，从右上角往左下角遍历，O(n)
    public int countLeqNumber(int[][] matrix, int n, int value) {
        int i = 0, j = n - 1;
        int count = 0;
        while (j >= 0 && i < n) {
            if (matrix[i][j] <= value) {
                count += j + 1;
                i++;
            } else {
                j--;
            }
        }
        return count;
    }
```

时间复杂度：O(n * logn * logX)，X为最大值与最小值的差