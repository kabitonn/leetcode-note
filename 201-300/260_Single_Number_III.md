# 260. Single Number III(M)

[260. 只出现一次的数字 III](https://leetcode-cn.com/problems/single-number-iii/)

## 题目描述(中等)

Given an array of numbers nums, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once.

Example:
```
Input:  [1,2,1,3,2,5]
Output: [3,5]
```
**Note**:

1. The order of the result is not important. So in the above example, [5, 3] is also correct.
2. Your algorithm should run in linear runtime complexity. Could you implement it using only constant space complexity?



## 思路

- 计数
- 位运算操作

## 解决方法

### 计数

```java
    public int[] singleNumber(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int n : nums) {
            map.put(n, map.getOrDefault(n, 0) + 1);
        }
        int[] onceNum = new int[2];
        int index = 0;
        for (int key : map.keySet()) {
            if (map.get(key) == 1) {
                onceNum[index++] = key;
                if (index == 2) {
                    break;
                }
            }
        }
        return onceNum;
    }

```

### 位运算

遍历数组获取全体异或后的结果，最后结果是两个只出现一次的数的异或结果，该结果上为1的bit位为两个数在该位上取不同值，根据这个可以将数组划分为两组(有一个数组每个数字都出现两次，有一个数字只出现了一次，求出该数字)，两组的异或结果即为只出现一次的两个数


mask为xor最右非0比特位，即两个数比特位不同的最小值

x的相反数=x的反码+1

某一个值的取反值加1后，在进行按位与操作，所得位恰好为初始值的最低位为所代表的值

```java
    public int[] singleNumber1(int[] nums) {
        int[] onceNum = new int[2];
        int xor = 0;
        for (int n : nums) {
            xor ^= n;
        }
        int mask = xor;
        //获取区分两个唯一数的最右非0比特位所代表的值
        mask &= (-mask);
        //mask &= (~mask) + 1;
        for (int n : nums) {
            if ((n & mask) == 0) {
                onceNum[0] ^= n;
            } else {
                onceNum[1] ^= n;
            }
        }
        return onceNum;
    }
```


mask为xor最右非0比特位，即两个数比特位不同的最小值

```java
    public int[] singleNumber1_1(int[] nums) {
        int[] onceNum = new int[2];
        int xor = 0;
        for (int n : nums) {
            xor ^= n;
        }
        int mask = 1;
        while ((xor & mask) == 0) {
            mask <<= 1;
        }
        for (int n : nums) {
            if ((n & mask) == 0) {
                onceNum[0] ^= n;
            } else {
                onceNum[1] ^= n;
            }
        }
        return onceNum;
    }
```