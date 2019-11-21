# 217. Contains Duplicate(E)
[217. Contains Duplicate](https://leetcode-cn.com/problems/contains-duplicate/)

## 题目描述(简单)

Given an array of integers, find if the array contains any duplicates.

Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

Example 1:
```
Input: [1,2,3,1]
Output: true
```
Example 2:
```
Input: [1,2,3,4]
Output: false
```
Example 3:
```
Input: [1,1,1,3,3,4,3,2,4,2]
Output: true
```
## 思路

- 哈希
- 排序比较

## 解决方法

### 哈希


```java
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for(int n:nums) {
        	if(set.contains(n)) {return true;}
        	set.add(n);
        }
        return false;
    }
```



### 排序比较相邻


```java
    public boolean containsDuplicate1(int[] nums) {
        Arrays.sort(nums);
        for(int i=1;i<nums.length;i++) {
        	if(nums[i]==nums[i-1]) {return true;}
        }
        return false;
    }
```



