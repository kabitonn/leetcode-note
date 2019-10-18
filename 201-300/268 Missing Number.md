## [268. Missing Number](https://leetcode-cn.com/problems/missing-number/)

## 1. 题目描述(简单)

Given an array containing n distinct numbers taken from 0, 1, 2, ..., n, find the one that is missing from the array.

Example 1:
```
Input: [3,0,1]
Output: 2
```
Example 2:
```
Input: [9,6,4,2,3,5,7,0,1]
Output: 8
```
**Note:**
- Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?


## 2. 思路



## 3. 解决方法

### 3.1 排序


```java
    public int missingNumber(int[] nums) {
        Arrays.sort(nums);
        int i=0;
        for(;i<nums.length;i++) {
        	if(nums[i]!=i) {
        		break;
        	}
        }
        return i;
    }
```



### 3.2 求总合

差值即为missing

```java
    public int missingNumber(int[] nums) {
        int sum = 0;
        for(int n:nums) {
        	sum+=n;
        }
        return (nums.length+1)*nums.length/2-sum;
    }
```


### 3.3 索引和值异或运算

missing为缺少的值的索引

```java
    public int missingNumber2(int[] nums) {
        int missing = nums.length;
        for (int i = 0; i < nums.length; i++) {
            missing ^= i ^ nums[i];
        }
        return missing;
    }
```



