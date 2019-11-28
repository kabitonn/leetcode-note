# 401. Binary Watch(E)

[401. 二进制手表](https://leetcode-cn.com/problems/binary-watch/)

## 题目描述(简单)

二进制手表顶部有 4 个 LED 代表**小时（0-11）**，底部的 6 个 LED 代表**分钟（0-59）**。

每个 LED 代表一个 0 或 1，最低位在右侧。


![](../ass/../assets/leetcode-note/401-500/401-p-1.png)

例如，上面的二进制手表读取 “3:25”。

给定一个非负整数 n 代表当前 LED 亮着的数量，返回所有可能的时间。

案例:
```
输入: n = 1
返回: ["1:00", "2:00", "4:00", "8:00", "0:01", "0:02", "0:04", "0:08", "0:16", "0:32"]
``` 

注意事项:

- 输出的顺序没有要求。
- 小时不会以零开头，比如 “01:00” 是不允许的，应为 “1:00”。
- 分钟必须由两位数组成，可能会以零开头，比如 “10:2” 是无效的，应为 “10:02”。


## 思路

- 暴力遍历
- 分治
- 递归回溯

## 解决方法

### 暴力遍历所有时间

```java
    public List<String> readBinaryWatch(int num) {
        List<String> list = new ArrayList<>();
        for (int i = 0; i < 12; i++) {
            for (int j = 0; j < 60; j++) {
                if (Integer.bitCount(i) + countOne(j) == num) {
                    String str = i + ":" + (j < 10 ? "0" + j : j);
                    list.add(str);
                }
            }
        }
        return list;
    }

    public int countOne(int n) {
        int count = 0;
        while (n != 0) {
            n = n & n - 1;
            count++;
        }
        return count;
    }
```

### 分治

```java
    public List<String> readBinaryWatch1(int num) {
        Map<Integer, List<Integer>> hourMap = bitMap(12);
        Map<Integer, List<Integer>> minuteMap = bitMap(60);
        List<String> list = new ArrayList<>();
        for (int i = 0; i <= num; i++) {
            if (!hourMap.containsKey(i) || !minuteMap.containsKey(num - i)) {
                continue;
            }
            for (int hour : hourMap.get(i)) {
                for (int minute : minuteMap.get(num - i)) {
                    list.add(String.format("%d:%02d", hour, minute));
                }
            }
        }
        return list;
    }

    public Map<Integer, List<Integer>> bitMap(int num) {
        Map<Integer, List<Integer>> map = new HashMap<>();
        for (int i = 0; i < num; i++) {
            int count = Integer.bitCount(i);
            if (!map.containsKey(count)) {
                map.put(count, new ArrayList<>());
            }
            map.get(count).add(i);
        }
        return map;
    }
```

### 递归回溯

```java
    int[] value = {1, 2, 4, 8, 1, 2, 4, 8, 16, 32};

    public List<String> readBinaryWatch2(int num) {
        int[] pos = new int[value.length];
        List<String> list = new ArrayList<>();
        backtrack(num, pos, 0, list);
        Collections.sort(list);
        return list;
    }

    public void backtrack(int num, int[] pos, int index, List<String> list) {
        if (num == 0) {
            int hour = 0, minute = 0;
            for (int i = 0; i < 4; i++) {
                hour += value[i] * pos[i];
            }
            if (hour >= 12) {
                return;
            }
            for (int i = 4; i < 10; i++) {
                minute += value[i] * pos[i];
            }
            if (minute >= 60) {
                return;
            }
            list.add(String.format("%d:%02d", hour, minute));
            return;
        }
        for (int i = index; i <= pos.length - num; i++) {
            pos[i]++;
            backtrack(num - 1, pos, i + 1, list);
            pos[i]--;
        }
    }
```