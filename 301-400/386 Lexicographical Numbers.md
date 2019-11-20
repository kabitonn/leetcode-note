# 386. Lexicographical Numbers(M)

[386. 字典序排数](https://leetcode-cn.com/problems/lexicographical-numbers/)

## 题目描述(中等)

给定一个整数 n, 返回从 1 到 n 的字典顺序。

例如，

给定 n =13，返回 `[1,10,11,12,13,2,3,4,5,6,7,8,9]` 。

请尽可能的优化算法的时间复杂度和空间复杂度。 输入的数据 n 小于等于 5,000,000。


## 思路

- 字符串排序
- 字典序生成序列

## 解决方法

### 字符串排序

```java
    public List<Integer> lexicalOrder(int n) {
        String[] nums = new String[n];
        for (int i = 0; i < n; i++) {
            nums[i] = String.valueOf(i + 1);
        }
        Arrays.sort(nums);
        List<Integer> list = new ArrayList<>();
        for (String num : nums) {
            list.add(Integer.valueOf(num));
        }
        return list;
    }

```

### 字典序递归生成DFS

```java
    public List<Integer> lexicalOrder1(int n) {
        List<Integer> list = new ArrayList<>();
        for (int i = 1; i <= 9; i++) {
            lexicalOrder1(n, i, list);
        }
        return list;
    }

    public void lexicalOrder1(int n, int min, List<Integer> list) {
        if (min > n) {
            return;
        }
        list.add(min);
        for (int i = 0; i <= 9; i++) {
            lexicalOrder1(n, min * 10 + i, list);
        }
    }
```

### 字典序模拟递归迭代生成

```java
    public List<Integer> lexicalOrder2(int n) {
        Stack<Integer> stack = new Stack<>();
        for (int i = Math.min(n, 9); i >= 1; i--) {
            stack.push(i);
        }
        List<Integer> list = new ArrayList<>();
        while (!stack.isEmpty()) {
            int num = stack.pop();
            list.add(num);
            for (int i = 9; i >= 0; i--) {
                int tmp = num * 10 + i;
                if (tmp <= n) {
                    stack.push(tmp);
                }
            }
        }
        return list;
    }
```

### 字典序迭代生成

```java
    public List<Integer> lexicalOrder3(int n) {
        List<Integer> list = new ArrayList<>(n);
        int num = 1;
        for (int i = 0; i < n; i++) {
            list.add(num);
            if (num * 10 <= n) {
                num *= 10;
            } else {
                if (num >= n) {
                    num /= 10;
                }
                num++;
                while (num % 10 == 0) {
                    num /= 10;
                }
            }
        }
        return list;
    }
```