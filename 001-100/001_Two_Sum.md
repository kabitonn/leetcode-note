# 001. Two Sum\(E\)

[001. Two Sum](https://leetcode-cn.com/problems/two-sum/)

## 题目描述\(简单\)

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

Example:

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

## 思路

1. 暴力法-双重循环遍历
2. 保存已经遍历的键值对

## 解决方法

### 暴力法-双重循环

双重循环，遍历所有情况判断相加和是否等于目标target，若符合则返回\(因为唯一存在\)。

```java
    public int[] twoSum(int[] nums, int target) {
        for(int i=0;i<nums.length;i++) {
            for(int j=i+1;j<nums.length;j++) {
                if ((nums[i]+nums[j]) == target) {
                    return new int[] {i,j};
                }
            }
        }
        return null;
    }
```

时间复杂度: $ O(n^2) $ 

空间复杂度：O\(1\)

### 两遍哈希

上述解法中，第二层 for 循环无非是遍历所有的元素，看哪个元素等于 差值 ，时间复杂度为 O\(n\)。  
不用遍历就可以找到元素里有没有等于差值的，采用HashMap

第一遍遍历将所有值作为key，索引作为value存入map，第二遍遍历判断当前值与目标target之间的差值是否存在，并且该值不为当前元素\(每个值只能用一次\)，若找到即可返回\(因为唯一存在\)。

```java
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int i=0;i<nums.length;i++) {
            map.put(nums[i], i);
        }
        for(int i=0;i<nums.length;i++) {
            int complement = target - nums[i];
            if (map.containsKey(complement) && map.get(complement) != i) {
                return new int[] { i, map.get(complement) };
            }
        }
        return null;
    }
```

时间复杂度O\(n\)  

空间复杂度：空间换时间， 开辟了一个 hash table ，空间复杂度变为 O\(n\)

### 一遍哈希

一次遍历过程中，若当前元素与目标target之间的差值存在\(不需判断为当前元素，因为当前元素还未放入map\)，即可返回\(唯一存在\)，若不存在，将当前元素值和索引作为key、value存入map;

```java
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int i=0;i<nums.length;i++) {
            int complement = target - nums[i];
            if (map.containsKey(complement) && map.get(complement) != i) {
                return new int[] { i, map.get(complement) };
            }
            map.put(nums[i], i);
        }

        return null;
    }
```

时间复杂度O\(n\)

