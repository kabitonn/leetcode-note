# 033. Search in Rotated Sorted Array(M)
[033. Search in Rotated Sorted Array](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

## 题目描述\(中等\)

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

\(i.e., \[0,1,2,4,5,6,7\] might become \[4,5,6,7,0,1,2\]\).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

Your algorithm's runtime complexity must be in the order of O(log (n)).

Example 1:

```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

Example 2:

```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

## 思路

## 解决方法

### 遍历

```java
    public int search(int[] nums, int target) {
        for(int i=0;i<nums.length;i++) {
            if(nums[i]==target)
                return i;
        }
        return -1;
    }
```
时间复杂度：O(n)。

空间复杂度：O(1)。



### 两段二分

我们每次只关心 mid 的值，所以 mid 要么就是 nums [ mid ]，要么就是 inf 或者 -inf。

什么时候是 nums [ mid ] 呢？

当 nums [ mid ] 和 target 在同一段里边。

- 怎么判断 nums [ mid ] 和 target 在同一段？
	- 把 nums [ mid ] 和 target 同时与 nums [ 0 ] 比较，如果它俩都大于 nums [ 0 ] 或者都小于 nums [ 0 ]，那么就代表它俩在同一段。例如[ 4 5 6 7 1 2 3]，如果 target = 5，此时数组看做 [ 4 5 6 7 inf inf inf ]。nums [ mid ] = 7，target > nums [ 0 ]，nums [ mid ] > nums [ 0 ]，所以它们在同一段 nums [ mid ] = 7，不用变化。

- 怎么判断 nums [ mid ] 和 target 不在同一段？

	- 把 nums [ mid ] 和 target 同时与 nums [ 0 ] 比较，如果它俩一个大于 nums [ 0 ] 一个小于 nums [ 0 ]，那么就代表它俩不在同一段。例如[ 4 5 6 7 1 2 3]，如果 target = 2，此时数组看做 [ - inf - inf - inf - inf 1 2 3]。nums [ mid ] = 7，target < nums [ 0 ]，nums [ mid ] > nums [ 0 ]，一个大于，一个小于，所以它们不在同一段 nums [ mid ] = - inf，变成了负无穷大。

```java
    public int search(int[] nums, int target) {
        int low = 0;
        int high = nums.length-1;
        while(low<=high) {
            int mid = (low+high)/2;
            int num ;
            if((nums[mid]<nums[0])==(target<nums[0])) {
                num = nums[mid];
            }
            else {
                num = nums[0]>target?Integer.MIN_VALUE:Integer.MAX_VALUE;
            }
            if(num>target) {
                high = mid-1;
            }
            else if (num<target) {
                low = mid+1;
            }
            else {
                return mid;
            }
        }
        return -1;
    }
```
### 判断有序片段

先找到哪一段是有序的 (只要判断端点即可)，然后看 target 在不在这一段里，如果在，那么就把另一半丢弃。如果不在，那么就把这一段丢弃。

```java
	public int search(int[] nums, int target) {
		int start = 0;
        int end = nums.length - 1;
        while (start <= end) {
            int mid = (start + end) / 2;
            if (nums[mid] == target) {
                return mid;
            }
            //左半段是有序的
            if (nums[start] <= nums[mid]) {
                if (nums[mid] > target && nums[start] <= target) {
                    end = mid - 1;
                } else {
                    start = mid + 1;
                }
            }
            //右半段是有序的
            else {
                if (nums[end] >= target && nums[mid] < target) {
                    start = mid + 1;
                } else {
                    end = mid - 1;
                }
            }

        }
        return -1;
	}
```



### 确认旋转点后二分

求最小值的偏移点
比较中点和端点值的情况
- mid 和 start 比较
  - mid > start: 最小值在左半部分。
  - mid < start：最小值在左半部分。
  
  无论大于小于，最小值都在左半部分，所以 mid 和 start 比较是不可取的
- mid 和 end 比较
  - mid < end：最小值在左半部分(包括mid)。
  - mid > end：最小值在右半部分。

  所以我们只需要把 mid 和 end 比较，mid < end 丢弃右半部分(更新 end = mid），mid > end 丢弃左半部分（更新 start = mid + 1）。直到 end 等于 start 时候结束就可以了。 
  
求最大值的偏移点
- mid 和 end 比较
  - mid < end：最大值在右半部分。
  - mid > end：最大值在右半部分。

  无论大于小于，最大值都在右半部分，所以 mid 和 end比较是不可取的
- mid 和 start 比较
  - mid > start: 最大值在右半部分(包括mid)。
  - mid < start：最大值在左半部分。

  所以我们只需要把 mid 和 start比较(取mid时取右端点)，mid < start 丢弃右半部分(更新 end = mid -1 ），mid < start丢弃左半部分（更新 start = mid ）。直到 end 等于 start 时候结束就可以了。 



```java
	public int search(int[] nums, int target) {
		int bias = getBiasByMax(nums);
		int start = 0;
		int end = nums.length-1;
		while(start<=end) {
			int mid = (start+end)/2;
			int trueMid = (mid+bias)%nums.length;
			if(nums[trueMid] == target)
				return trueMid;
			else if(nums[trueMid] < target) {
				start = mid+1;
			}
			else if(nums[trueMid] > target){
				end = mid -1;
			}
		}
		return -1;
	}
	public int getBiasByMin(int[] nums) {
		int start = 0;
		int end = nums.length-1;
		while (start<end) {
			int mid = (start+end)/2;
			if(nums[mid]>nums[end]) {
				start = mid+1;
			}
			else {
				end = mid;
			}
		}
		return start;
	}
	public int getBiasByMax(int[] nums) {
		int start = 0;
		int end = nums.length-1;
		while (start<end) {
			int mid = (start+end+1)/2;
			if(nums[mid]>nums[start]) {
				start = mid;
			}
			else {
				end = mid-1;
			}
		}
		return end+1;
	}
```

时间复杂度：O(log(n))。

空间复杂度：O(1)。
