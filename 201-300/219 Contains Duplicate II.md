# 219. Contains Duplicate II
[219. Contains Duplicate II](https://leetcode-cn.com/problems/contains-duplicate-ii/)

## 题目描述(简单)

Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that nums[i] = nums[j] and the absolute difference between i and j is at most k.

Example 1:
```
Input: nums = [1,2,3,1], k = 3
Output: true
```
Example 2:
```
Input: nums = [1,0,1,1], k = 1
Output: true
```
Example 3:
```
Input: nums = [1,2,3,1,2,3], k = 2
Output: false
```
## 思路

## 解决方法

### 遍历


```java
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        for(int i=0;i<nums.length;i++) {
            for(int j=i+1;j<=i+k&&j<nums.length;j++) {
                if(nums[i]==nums[j]) {return true;}
            }
        }
        return false;
    }
```
时间复杂度：$$O(n * \min(k,n))$$每次搜索都要花费$$ O(\min(k, n))$$ 的时间，哪怕k比n大，一次搜索中也只需比较 n 次。

空间复杂度：O(1)
### HashMap


```java
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Map<Integer,Integer> map = new HashMap<>();
        for(int i=0;i<nums.length;i++) {
            if(map.containsKey(nums[i])&&(i-map.get(nums[i])<=k)){
                return true;
            }
            else {
                map.put(nums[i], i);
            }
        }
        return false;
    }
```
时间复杂度：O(n)，这里 n 是数组的元素个数，算法遍历了一次数组。
空间复杂度：O(n)，这里使用了哈希表存储已经出现的数，可以说是以空间换时间了。


### HashSet 滑动窗口

遍历数组，对于每个元素做以下操作：
- 在散列表中搜索当前元素，如果找到了就返回 true。
- 在散列表中插入当前元素。
- 如果当前散列表的大小超过了 k， 删除散列表中最旧的元素。


```java
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Set<Integer> set = new HashSet<>();
        for(int i=0;i<nums.length;i++) {
            if(set.add(nums[i])) {
                if(set.size()>k) {
                    set.remove(nums[i-k]);
                }
            }
            else {
                return true;
            }
        }
        return false;
    }
```
时间复杂度：O(n) 我们会做 n 次 搜索，删除，插入 操作，每次操作都耗费常数时间。

空间复杂度：O(min(n, k)) 开辟的额外空间取决于散列表中存储的元素的个数，也就是滑动窗口的大小 




