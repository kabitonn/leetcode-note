
# 451. Sort Characters By Frequency(M)

[451. 根据字符出现频率排序](https://leetcode-cn.com/problems/sort-characters-by-frequency/)

## 题目描述(中等)

给定一个字符串，请将字符串里的字符按照出现的频率降序排列。

示例 1:
```
输入:
"tree"

输出:
"eert"

解释:
'e'出现两次，'r'和't'都只出现一次。
因此'e'必须出现在'r'和't'之前。此外，"eetr"也是一个有效的答案。
```

示例 2:
```
输入:
"cccaaa"

输出:
"cccaaa"

解释:
'c'和'a'都出现三次。此外，"aaaccc"也是有效的答案。
注意"cacaca"是不正确的，因为相同的字母必须放在一起。
```

示例 3:
```
输入:
"Aabb"

输出:
"bbAa"

解释:
此外，"bbaA"也是一个有效的答案，但"Aabb"是不正确的。
注意'A'和'a'被认为是两种不同的字符。
```

## 思路

- 计数 排序

## 解决方法

### 计数 + 排序

统计每个字符出现频率，根据频率对字符进行排序

```java
    public String frequencySort(String s) {
        Map<Character, Integer> count = new HashMap<>();
        for (char c : s.toCharArray()) {
            if (!count.containsKey(c)) {
                count.put(c, 0);
            }
            count.put(c, count.get(c) + 1);
        }
        int n = count.size();
        Character[] chars = new Character[n];
        count.keySet().toArray(chars);
        Arrays.sort(chars, new Comparator<Character>() {
            @Override
            public int compare(Character o1, Character o2) {
                return count.get(o1) - count.get(o2);
            }
        });
        StringBuilder sb = new StringBuilder();
        for (int i = n - 1; i >= 0; i--) {
            for (int j = 0; j < count.get(chars[i]); j++) {
                sb.append(chars[i]);
            }
        }
        return sb.toString();
    }

```


时间复杂度 O(n log(n))

空间复杂度 O(n)

### 计数 优先队列

统计每个字符出现频率，根据频率对字符进行优先排序

```java
    public String frequencySort1(String s) {
        int[] count = new int[128];
        for (char c : s.toCharArray()) {
            count[c]++;
        }
        PriorityQueue<Character> priorityQueue = new PriorityQueue<>(new Comparator<Character>() {
            @Override
            public int compare(Character o1, Character o2) {
                return count[o2] - count[o1];
            }
        });
        for (int i = 0; i < count.length; i++) {
            if (count[i] == 0) {
                continue;
            }
            priorityQueue.add((char) i);
        }
        StringBuilder sb = new StringBuilder();
        while (!priorityQueue.isEmpty()) {
            char c = priorityQueue.poll();
            for (int i = 0; i < count[c]; i++) {
                sb.append(c);
            }
        }
        return sb.toString();
    }
```

时间复杂度 O(n log(n))

空间复杂度 O(n)

### 计数 + 桶排序

```java
    public String frequencySort3(String s) {
        int[] count = new int[128];
        int max = 0;
        for (char c : s.toCharArray()) {
            count[c]++;
            max = Math.max(max, count[c]);
        }
        List<Character>[] buckets = new List[max + 1];
        for (int c = 0; c < count.length; c++) {
            if (buckets[count[c]] == null) {
                buckets[count[c]] = new ArrayList<>();
            }
            buckets[count[c]].add((char) c);
        }
        StringBuilder sb = new StringBuilder();
        for (int i = max; i > 0; i--) {
            if (buckets[i] != null) {
                for (char c : buckets[i]) {
                    for (int k = 0; k < i; k++) {
                        sb.append(c);
                    }
                }
            }
        }
        return sb.toString();
    }
```

时间复杂度 O(n)

空间复杂度 O(n)

### 计数 + 双数组

维护两个数组
- count[c]:字符c出现频率
- copy[j] :字符j频率，未添加进结果的j

依据当前最高频的频率找到字符j,将字符j添加进结果count[j]次；将count[j]置零

```java
    public String frequencySort2(String s) {
        int[] count = new int[128];
        for (char c : s.toCharArray()) {
            count[c]++;
        }
        int[] copy = count.clone();
        Arrays.sort(count);
        int n = count.length;
        StringBuilder sb = new StringBuilder();
        for (int i = n - 1; i >= 0; i--) {
            if (count[i] == 0) {
                continue;
            }
            for (int j = 0; j < n; j++) {
                if (count[i] != copy[j]) {
                    continue;
                }
                while (copy[j]-- > 0) {
                    sb.append((char) j);
                }
            }
        }
        return sb.toString();
    }
```

时间复杂度 O(n^2)

空间复杂度 O(n)