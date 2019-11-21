# 347. Top K Frequent Elements(M)

[347. 前 K 个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/)

## 题目描述(中等)

给定一个非空的整数数组，返回其中出现频率前 k 高的元素。

示例 1:
```
输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
```
示例 2:
```
输入: nums = [1], k = 1
输出: [1]
```
**说明**：
- 你可以假设给定的 k 总是合理的，且 1 ≤ k ≤ 数组中不相同的元素的个数。
- 你的算法的时间复杂度必须优于 O(n log n) , n 是数组的大小。


## 思路

- 统计频率 + 排序

## 解决方法

### 统计频率排序

```java
    public List<Integer> topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> countMap = new HashMap<>();
        for (int num : nums) {
            countMap.put(num, countMap.getOrDefault(num, 0) + 1);
        }
        int n = countMap.size();
        int[][] frequence = new int[n][2];
        int i = 0;
        for (int key : countMap.keySet()) {
            frequence[i][0] = key;
            frequence[i][1] = countMap.get(key);
            i++;
        }
        /*
        Arrays.sort(frequence, new Comparator<int[]>() {
                @Override
                public int compare(int[] o1, int[] o2) {
                    return o2[1] - o1[1];
                }
            }
        );
        */
        Arrays.sort(frequence, (o1, o2) -> o2[1] - o1[1]);
        List<Integer> list = new ArrayList<>();
        for (i = 0; i < k; i++) {
            list.add(frequence[i][0]);
        }
        return list;
    }
```

### 统计频率求第k高的频率

```java
    public List<Integer> topKFrequent1(int[] nums, int k) {
        Map<Integer, Integer> countMap = new HashMap<>();
        for (int num : nums) {
            countMap.put(num, countMap.getOrDefault(num, 0) + 1);
        }
        int n = countMap.size();
        int[] frequence = new int[n];
        int i = 0;
        for (int key : countMap.keySet()) {
            frequence[i++] = countMap.get(key);
        }
        Arrays.sort(frequence);
        List<Integer> list = new ArrayList<>();
        int limit = frequence[n - k];
        for (int key : countMap.keySet()) {
            if (countMap.get(key) >= limit) {
                list.add(key);
            }
        }
        return list;
    }
```

### 统计频率桶排序

```java
    public List<Integer> topKFrequent2(int[] nums, int k) {
        Map<Integer, Integer> countMap = new HashMap<>();
        for (int num : nums) {
            countMap.put(num, countMap.getOrDefault(num, 0) + 1);
        }
        int n = nums.length;
        List<Integer>[] lists = new List[n + 1];
        for (int key : countMap.keySet()) {
            int f = countMap.get(key);
            if (lists[f] == null) {
                lists[f] = new ArrayList<>();
            }
            lists[f].add(key);
        }
        List<Integer> list = new ArrayList<>();
        for (int i = nums.length; i >= 0 && list.size() < k; i--) {
            if (lists[i] != null) {
                list.addAll(lists[i]);
            }
        }
        return list;
    }
```

### 小根堆

```java
    public List<Integer> topKFrequent3_1(int[] nums, int k) {
        Map<Integer, Integer> countMap = new HashMap<>();
        for (int num : nums) {
            countMap.put(num, countMap.getOrDefault(num, 0) + 1);
        }
        PriorityQueue<Integer> minHeap = new PriorityQueue<>(k + 1, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return countMap.get(o1) - countMap.get(o2);
            }
        });
        for (int key : countMap.keySet()) {
            minHeap.add(key);
            if (minHeap.size() > k) {
                minHeap.poll();
            }
        }
        return new ArrayList<>(minHeap);
    }
```

### TreeMap

```java
    public List<Integer> topKFrequent4(int[] nums, int k) {
        Map<Integer, Integer> countMap = new HashMap<>();
        for (int num : nums) {
            countMap.put(num, countMap.getOrDefault(num, 0) + 1);
        }
        TreeMap<Integer, Integer> treeMap = new TreeMap<>(new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return countMap.get(o2) - countMap.get(o1) > 0 ? 1 : -1;
            }
        });
        treeMap.putAll(countMap);
        List<Integer> list = new ArrayList<>();
        for (int key : treeMap.keySet()) {
            list.add(key);
            if (list.size() == k) {
                break;
            }
        }
        return list;
    }
```