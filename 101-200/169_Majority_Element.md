# 169. Majority Element(E)
[169. 求众数](https://leetcode-cn.com/problems/majority-element/)

## 题目描述(简单)

Given an array of size n, find the majority element. The majority element is the element that appears more than $$ \lfloor n / 2 \rfloor $$ times.

You may assume that the array is non-empty and the majority element always exist in the array.

Example 1:
```
Input: [3,2,3]
Output: 3
```
Example 2:
```
Input: [2,2,1,1,1,2,2]
Output: 2
```


## 思路

- 排序中位数
- 哈希计数
- Boyer Moore投票

## 解决方法

### 排序中位数


```java
    public int majorityElement(int[] nums) {
    	Arrays.sort(nums);
        return nums[nums.length/2];
    }
```


### 哈希表


```java
    public int majorityElement1(int[] nums) {
    	Map<Integer, Integer> map = new HashMap<>();
    	for(int n :nums) {
    		int frequence = map.getOrDefault(n, 0);
    		map.put(n, frequence+1);
    		if(map.get(n)>nums.length/2) {
    			return n;
    		}
    	}
    	return -1;
    }
```

### Boyer Moore投票法

如果我们把众数记为 +1，把其他数记为 -1 ，将它们全部加起来，显然和大于 0 ，从结果本身我们可以看出众数比其他数多。

```java
    public int majorityElement2(int[] nums) {
    	int candidate=nums[0],count=1,i=1;
        for(;i<nums.length;i++)
        {
            if(candidate==nums[i])
                count++;
            else if(--count==0)
                candidate=nums[i+1];
        }   
        return candidate;
	}
```


```java
    public int majorityElement(int[] nums) {
		Integer candidate=nums[0];
		int count=0;
		for(int n:nums){
			if(count==0){
				candidate = n;
			}
			count += n==candidate?1:-1;
		}
		return candidate;
	}
```


时间复杂度：O(n) Boyer-Moore 算法严格执行 n 次循环，时间复杂度是线性时间

空间复杂度：O(1) Boyer-Moore 只需要常数级别的额外空间。



