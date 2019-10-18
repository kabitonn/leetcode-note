# 167. Two Sum II - Input array is sorted(E)
[167. Two Sum II - Input array is sorted](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)

## 题目描述(简单)

Given an array of integers that is already sorted in ascending order, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2.

**Note**:

- Your returned answers (both index1 and index2) are not zero-based.
- You may assume that each input would have exactly one solution and you may not use the same element twice.

Example:
```
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.
```


## 思路

## 解决方法

### 遍历


```java
    public int[] twoSum(int[] numbers, int target) {
        for(int i=0;i<numbers.length-1;i++) {
        	for(int j=i+1;j<numbers.length;j++) {
        		int sum = numbers[i]+numbers[j];
        		if(sum==target) {
        			return new int[] {i+1,j+1};
        		}
        		else if (sum>target) {
					break;
				}
        	}
        }
        return new int[]{};
    }
```


### 双指针


```java
    public int[] twoSum(int[] numbers, int target) {
    	int i = 0;
    	int j = numbers.length-1;
    	while(i<j) {
    		int sum = numbers[i]+numbers[j];
    		if(sum==target) {
    			return new int[] {i+1,j+1};
    		}
    		else if (sum>target) {
				j--;
			}
    		else if (sum<target) {
				i++;
			}
    	}
    	return new int[]{};
    }
```

