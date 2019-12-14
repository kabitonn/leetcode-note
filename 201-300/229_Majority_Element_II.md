# 229. Majority Element II(M)

[](https://leetcode-cn.com/problems/majority-element-ii/)

## 题目描述(中等)

Given an integer array of size n, find all elements that appear more than $\lfloor n/3 \rfloor$ times.

**Note**: The algorithm should run in linear time and in O(1) space.

Example 1:
```
Input: [3,2,3]
Output: [3]
```
Example 2:
```
Input: [1,1,1,3,3,2,2,2]
Output: [1,2]
```


## 思路


## 解决方法

### 计数


```java
    public List<Integer> majorityElement(int[] nums) {
        List<Integer> list = new ArrayList<>();
        Map<Integer, Integer> map = new HashMap<>();
        for (int n : nums) {
            map.put(n, map.getOrDefault(n, 0) + 1);
        }
        int times = nums.length / 3;
        for (int n : map.keySet()) {
            if (map.get(n) > times) {
                list.add(n);
            }
        }
        return list;
    }

```

### 投票 + 验证

超过n/3的数最多只能有两个。先选出两个候选人A,B。 遍历数组，分三种情况：

1. 如果投A(当前元素等于A)，A的票数++;

2. 如果投B(当前元素等于B)，B的票数++；

3. 如果A,B都不投(即当前与A，B都不相等),那么检查此时A或B的票数是否减为0：

    1. 如果为0,则当前元素成为新的候选人；

    2. 如果A,B两个人的票数都不为0，那么A,B两个候选人的票数均减一；


满足条件的数最多为两个，找出两个候选数进行投票，进行两次遍历，第一次遍历找到两个候选数，第二次遍历统计出现次数，最后验证是否满足出现次数。

```java
    public List<Integer> majorityElement1(int[] nums) {
        List<Integer> list = new ArrayList<>();
        Integer candidateA = null, candidateB = null;
        int countA = 0, countB = 0;
        for (int n : nums) {
            if (candidateA != null && n == candidateA) {
                countA++;
            } else if (candidateB != null && n == candidateB) {
                countB++;
            } else if (countA == 0) {
                candidateA = n;
                countA++;
            } else if (countB == 0) {
                candidateB = n;
                countB++;
            } else {
                countA--;
                countB--;
            }
        }
        countA = countB = 0;
        for (int n : nums) {
            if (n == candidateA) {
                countA++;
            } else if (n == candidateB) {
                countB++;
            }
        }
        if (countA > nums.length / 3) {
            list.add(candidateA);
        }
        if (countB > nums.length / 3) {
            list.add(candidateB);
        }
        return list;
    }
```