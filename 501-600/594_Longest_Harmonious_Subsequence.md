
# 594. Longest Harmonious Subsequence(E)
 
[594. 最长和谐子序列](https://leetcode-cn.com/problems/longest-harmonious-subsequence/)

## 题目描述(简单)

和谐数组是指一个数组里元素的最大值和最小值之间的差别正好是1。

现在，给定一个整数数组，你需要在所有可能的子序列中找到最长的和谐子序列的长度。

示例 1:
```
输入: [1,3,2,2,5,2,3,7]
输出: 5
原因: 最长的和谐数组是：[3,2,2,2,3].
```


## 思路

统计相差1的两种数的的个数和



## 解决方法

### 暴力遍历

```java
    public int findLHS2(int[] nums) {
        int len = 0;
        for (int i = 0; i < nums.length; i++) {
            int count = 0;
            boolean flag = false;
            for (int j = 0; j < nums.length; j++) {
                if (nums[j] == nums[i]) {
                    count++;
                } else if (nums[j] + 1 == nums[i]) {
                    count++;
                    flag = true;
                }
            }
            if (flag) {
                len = Math.max(count, len);
            }
        }
        return len;
    }

```

### 排序 遍历

```java
    public int findLHS(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        Arrays.sort(nums);
        int prev = nums[0];
        int sum = 0;
        int count1 = 0, count2 = 1;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] - 1 == prev) {
                count1 = count2;
                count2 = 0;
            } else if (nums[i] - 1 > prev) {
                count1 = 0;
                count2 = 0;
            }
            count2++;
            if (count1 != 0) {
                sum = Math.max(sum, count1 + count2);
            }
            prev = nums[i];
        }
        return sum;
    }
```

```java
    public int findLHS0(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        Arrays.sort(nums);
        int prev = nums[0];
        int sum = 0;
        int count1 = 0, count2 = 1;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] == prev) {
                count2++;
            } else {
                if (count1 != 0) {
                    sum = Math.max(sum, count1 + count2);
                }
                if (nums[i] - 1 == prev) {
                    count1 = count2;
                    count2 = 1;
                } else if (nums[i] - 1 > prev) {
                    count1 = 0;
                    count2 = 1;
                }
                prev = nums[i];
            }
        }
        if (count1 != 0) {
            sum = Math.max(sum, count1 + count2);
        }
        return sum;
    }
```

### 排序 双指针

两个指针表示两个相差为1的两个数边界索引

```java
    public int findLHS1(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        Arrays.sort(nums);
        int n = nums.length;
        int i = 0;
        int len = 0;
        for (int j = 1; j < n; j++) {
            while (nums[j] - nums[i] > 1) {
                i++;
            }
            if (nums[j] - nums[i] == 1) {
                len = Math.max(len, j - i + 1);
            }
        }
        return len;
    }
```

### 哈希映射

```java
    public int findLHS3(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        int len = 0;
        for (int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        for (int key : map.keySet()) {
            if (map.containsKey(key + 1)) {
                len = Math.max(len, map.get(key) + map.get(key + 1));
            }
        }
        return len;
    }
```

### 哈希映射 单次扫描

```java
    public int findLHS4(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        int len = 0;
        for (int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);
            if (map.containsKey(num + 1)) {
                len = Math.max(len, map.get(num) + map.get(num + 1));
            }
            if (map.containsKey(num - 1)) {
                len = Math.max(len, map.get(num) + map.get(num - 1));
            }
        }
        return len;
    }
```