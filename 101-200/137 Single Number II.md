# 137. Single Number II(M)


[137. 只出现一次的数字 II](https://leetcode-cn.com/problems/single-number-ii/)


## 题目描述(中等)

Given a **non-empty** array of integers, every element appears three times except for one, which appears exactly once. Find that single one.

**Note**:

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

Example 1:
```
Input: [2,2,3,2]
Output: 3
```
Example 2:
```
Input: [0,1,0,1,0,1,99]
Output: 99
```


## 思路

- 排序
- 计数
- 位运算


## 解决方法



### 排序

```java
    public int singleNumber(int[] nums) {
        Arrays.sort(nums);
        for (int i = 0; i < nums.length; i++) {
            if (i + 1 < nums.length && nums[i] == nums[i + 1]) {
                while (i + 1 < nums.length && nums[i] == nums[i + 1]) {
                    i++;
                }
            } else {
                return nums[i];
            }
        }
        return 0;
    }
```

### 计数

```java
    public int singleNumber1(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int n : nums) {
            Integer count = map.get(n);
            count = count == null ? 1 : count + 1;
            map.put(n, count);
        }
        for (Integer n : map.keySet()) {
            if (map.get(n) == 1) {
                return n;
            }
        }
        return 0;
    }
```

### 位运算
```java
    public int singleNumber2(int[] nums) {
        int single = 0;
        //考虑每一位
        for (int i = 0; i < 32; i++) {
            int count = 0;
            //考虑每一个数
            for (int j = 0; j < nums.length; j++) {
                //当前位是否是 1
                if ((nums[j] >>> i & 1) == 1) {
                    count++;
                }
            }
            //1 的个数是否是 3 的倍数
            if (count % 3 != 0) {
                single = single | 1 << i;
            }
        }
        return single;
    }

```



### 位运算 通解

```java
    public int singleNumber5(int[] nums) {
        int x1 = 0, x2 = 0, mask = 0;
        for (int i : nums) {
            x2 ^= x1 & i;
            x1 ^= i;
            mask = ~(x1 & x2);
            x2 &= mask;
            x1 &= mask;
        }
        return x1;
    }
```




