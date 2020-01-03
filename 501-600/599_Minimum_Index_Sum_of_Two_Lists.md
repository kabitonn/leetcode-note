
# 599. Minimum Index Sum of Two Lists(E)
 
[599. 两个列表的最小索引总和](https://leetcode-cn.com/problems/minimum-index-sum-of-two-lists/)

## 题目描述(简单)

## 思路

## 解决方法

### 暴力法

遍历每一种索引和的情况

!> 超时

```java
    public String[] findRestaurant(String[] list1, String[] list2) {
        int m = list1.length, n = list2.length;
        List<String> list = new ArrayList<>();
        for (int k = 0; k < m + n; k++) {
            for (int i = 0; i < k && i < m; i++) {
                for (int j = 0; i + j < k && j < n; j++) {
                    if (list1[i].equals(list2[j])) {
                        list.add(list1[i]);
                    }
                }
            }
            if (list.size() != 0) {
                break;
            }
        }
        return list.toArray(new String[0]);
    }
```

### 暴力法 优化

遍历索引和，获取最小索引来缩小范围

```java
    public String[] findRestaurant1(String[] list1, String[] list2) {
        int m = list1.length, n = list2.length;
        List<String> list = new ArrayList<>();
        int min = m + n;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n && i + j <= min; j++) {
                if (list1[i].equals(list2[j])) {
                    if (i + j < min) {
                        list = new ArrayList<>();
                        min = i + j;
                    }
                    list.add(list1[i]);
                }
            }
        }
        return list.toArray(new String[0]);
    }
```

### 哈希

```java
    public String[] findRestaurant2(String[] list1, String[] list2) {
        Map<String, Integer> map = new HashMap<>();
        int m = list1.length, n = list2.length;
        List<String> list = new ArrayList<>();
        int min = m + n;
        for (int i = 0; i < m; i++) {
            map.put(list1[i], i);
        }
        for (int j = 0; j < n && j <= min; j++) {
            if (map.containsKey(list2[j])) {
                int i = map.get(list2[j]);
                if (i + j == min) {
                    list.add(list1[i]);
                }
                if (i + j < min) {
                    list = new ArrayList<>();
                    min = i + j;
                    list.add(list1[i]);
                }
            }
        }
        return list.toArray(new String[0]);
    }
```