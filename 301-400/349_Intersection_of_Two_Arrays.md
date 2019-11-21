# 349. Intersection of Two Arrays(E)
[349. Intersection of Two Arrays](https://leetcode-cn.com/problems/intersection-of-two-arrays/)

## 题目描述(简单)

Given two arrays, write a function to compute their intersection.

Example 1:
```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]
```
Example 2:
```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
```
Note:

- Each element in the result must be unique.
- The result can be in any order.


## 思路

## 解决方法

### 排序


```java
    public int[] intersection(int[] nums1, int[] nums2) {
        Arrays.sort(nums1);
        Arrays.sort(nums2);
        Set<Integer> set = new HashSet<>();
        int i=0,j=0;
        while(i<nums1.length&&j<nums2.length) {
        	//while(i+1<nums1.length && nums1[i+1]==nums1[i]) i++;
            //while(j+1<nums2.length && nums2[j+1]==nums2[j]) j++;
        	if(nums1[i]<nums2[j]) {
        		i++;
        	}
        	else if (nums1[i]>nums2[j]) {
        		j++;
			}
        	else if (nums1[i]==nums2[j]) {
        		set.add(nums1[i]);
        		i++;
        		j++;
			}
        }
        int[] res = new int[set.size()];
        i = 0;
        for(Integer n:set) {
        	res[i++] = n;
        }
        return res;
    }
```

### 两个set


```java 
	public int[] intersection(int[] nums1, int[] nums2) {
		HashSet<Integer> set1 = new HashSet<Integer>();
		for (Integer n : nums1) set1.add(n);
		HashSet<Integer> set2 = new HashSet<Integer>();
		for (Integer n : nums2) set2.add(n);

		int size = set1.size()>set2.size()?set2.size():set1.size();
		int[] res = new int[size];
		int i = 0;
		for(Integer n:set1) {
			if(set2.contains(n)){res[i++] = n;}
		}
		return Arrays.copyOf(res, i);
	}
```



### HashSet 内置函数


```java
	public int[] intersection(int[] nums1, int[] nums2) {
		HashSet<Integer> set1 = new HashSet<Integer>();
		for (Integer n : nums1) set1.add(n);
		HashSet<Integer> set2 = new HashSet<Integer>();
		for (Integer n : nums2) set2.add(n);

		set1.retainAll(set2);
		int[] res = new int[set1.size()];
		int i = 0;
		for(Integer n:set1) {
			res[i++] = n;
		}
		return res;
	}
```

时间复杂度：一般情况下是 O(m+n)，最坏情况下是 $$O(m \times n)$$。
空间复杂度：最坏的情况是 O(m+n)，当数组中的元素全部不一样时。


