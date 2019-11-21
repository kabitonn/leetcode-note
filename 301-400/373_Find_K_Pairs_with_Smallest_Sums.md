# 373. Find K Pairs with Smallest Sums(M)

[373. 查找和最小的K对数字](https://leetcode-cn.com/problems/find-k-pairs-with-smallest-sums/)

## 题目描述(中等)

给定两个以升序排列的整形数组 nums1 和 nums2, 以及一个整数 k。

定义一对值 `(u,v)`，其中第一个元素来自 `nums1`，第二个元素来自 `nums2`。

找到和最小的 k 对数字 `(u1,v1), (u2,v2) ... (uk,vk)`。

示例 1:
```
输入: nums1 = [1,7,11], nums2 = [2,4,6], k = 3
输出: [1,2],[1,4],[1,6]
解释: 返回序列中的前 3 对数：
     [1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]
```
示例 2:
```
输入: nums1 = [1,1,2], nums2 = [1,2,3], k = 2
输出: [1,1],[1,1]
解释: 返回序列中的前 2 对数：
     [1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]
```
示例 3:
```
输入: nums1 = [1,2], nums2 = [3], k = 3 
输出: [1,3],[2,3]
解释: 也可能序列中所有的数对都被返回:[1,3],[2,3]
```
## 思路

- 排序
- 缩减可能组合

## 解决方法

### 大根堆

全部组合进入堆，size大于k后poll，最后留下的k个即为所求结果

```java
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        PriorityQueue<List<Integer>> maxHeap = new PriorityQueue<>(k + 1, new Comparator<List<Integer>>() {
            @Override
            public int compare(List<Integer> o1, List<Integer> o2) {
                int sum1 = o1.get(0) + o1.get(1);
                int sum2 = o2.get(0) + o2.get(1);
                return sum2 - sum1;
            }
        });
        int m = Math.min(nums1.length, k);
        int n = Math.min(nums2.length, k);
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                maxHeap.add(new ArrayList<>(Arrays.asList(nums1[i], nums2[j])));
                if (maxHeap.size() > k) {
                    maxHeap.poll();
                }
            }
        }
        return new ArrayList<>(maxHeap);
    }

```

时间复杂度：O(k\*klog(k))

### 数组排序

全部组合进行排序，前k个即为所求结果

```java
    public List<List<Integer>> kSmallestPairs0(int[] nums1, int[] nums2, int k) {
        int m = Math.min(nums1.length, k);
        int n = Math.min(nums2.length, k);
        int[][] array = new int[m * n][2];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                array[i * n + j][0] = nums1[i];
                array[i * n + j][1] = nums2[j];
            }
        }
        Arrays.sort(array, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                return (o1[0] + o1[1]) - (o2[0] + o2[1]);
            }
        });
        k = Math.min(k, array.length);
        List<List<Integer>> listList = new ArrayList<>();
        for (int i = 0; i < k; i++) {
            int[] num = array[i];
            listList.add(new ArrayList<>(Arrays.asList(num[0], num[1])));
        }
        return listList;
    }
```


时间复杂度：O(k\*klog(k\*k))

### 小根堆

```java
    public List<List<Integer>> kSmallestPairs2(int[] nums1, int[] nums2, int k) {
        int m = Math.min(nums1.length, k);
        int n = Math.min(nums2.length, k);
        if (m == 0 || n == 0) {
            return new ArrayList<>();
        }
        PriorityQueue<int[]> minHeap = new PriorityQueue<>(k * k, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                return (o1[0] + o1[1]) - (o2[0] + o2[1]);
            }
        });
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                minHeap.add(new int[]{nums1[i], nums2[j]});
            }
        }
        List<List<Integer>> listList = new ArrayList<>();
        k = Math.min(k, minHeap.size());
        for (int i = 0; i < k; i++) {
            int[] num = minHeap.poll();
            listList.add(new ArrayList<>(Arrays.asList(num[0], num[1])));
        }
        return listList;
    }
```

时间复杂度：O(k\*klog(k))



### 小根堆 + 缩减可能组合

- 首先加入最小的一些组合
- 弹出一个组合就加入一个稍微大一点的组合
- 直到弹出的个数符合要求


```java
    public List<List<Integer>> kSmallestPairs3(int[] nums1, int[] nums2, int k) {
        int m = Math.min(nums1.length, k);
        int n = Math.min(nums2.length, k);
        if (m == 0 || n == 0) {
            return new ArrayList<>();
        }
        PriorityQueue<int[]> minHeap = new PriorityQueue<>(k * 2, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                return (nums1[o1[0]] + nums2[o1[1]]) - (nums1[o2[0]] + nums2[o2[1]]);
            }
        });
        for (int i = 0; i < m; i++) {
            minHeap.add(new int[]{i, 0});
        }
        List<List<Integer>> listList = new ArrayList<>();
        k = Math.min(k, m * n);
        for (int i = 0; i < k; i++) {
            int[] index = minHeap.poll();
            listList.add(new ArrayList<>(Arrays.asList(nums1[index[0]], nums2[index[1]])));
            index[1]++;
            if (index[1] < n) {
                minHeap.add(index);
            }
        }
        return listList;
    }
```


时间复杂度：O(2klog(k))
