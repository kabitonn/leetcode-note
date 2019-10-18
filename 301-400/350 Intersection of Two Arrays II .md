## [350. Intersection of Two Arrays II](https://leetcode-cn.com/problems/intersection-of-two-arrays-ii/)

## 1. 题目描述(简单)

Given two arrays, write a function to compute their intersection.

Example 1:
```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2,2]
```
Example 2:
```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [4,9]
```
**Note:**

- Each element in the result should appear as many times as it shows in both arrays.
- The result can be in any order.

**Follow up:**

- What if the given array is already sorted? How would you optimize your algorithm?
- What if nums1's size is small compared to nums2's size? Which algorithm is better?
- What if elements of nums2 are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?


## 2. 思路

## 3. 解决方法

### 3.1 排序


```java
    public int[] intersect(int[] nums1, int[] nums2) {
    	Arrays.sort(nums1);
        Arrays.sort(nums2);
        int size = nums1.length>nums2.length?nums2.length:nums1.length;
		int[] res = new int[size];
        int i=0,j=0,index=0;
        while(i<nums1.length&&j<nums2.length) {
        	if(nums1[i]<nums2[j]) {
        		i++;
        	}
        	else if (nums1[i]>nums2[j]) {
        		j++;
			}
        	else if (nums1[i]==nums2[j]) {
        		res[index++] = nums1[i];
        		i++;
        		j++;
			}
        }

        return Arrays.copyOf(res, index);
    }
```



### 3.2 HashMap


```java
    public int[] intersect(int[] nums1, int[] nums2) {
        HashMap<Integer, Integer> map = new HashMap<>();
        int size = nums1.length > nums2.length ? nums2.length : nums1.length;
        int[] res = new int[size];
        int index = 0;
        for (int n : nums1) {
            int count = map.getOrDefault(n, 0);
            map.put(n, ++count);
        }
        for (int n : nums2) {
            if (map.containsKey(n)&&map.get(n)>0) {
                res[index++] = n;
                map.put(n, map.get(n)-1);
            }
        }
        return Arrays.copyOf(res, index);
    }
```


### 3.3 

