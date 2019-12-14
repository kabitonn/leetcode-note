# 015. 3Sum(M)
[015. 3Sum](https://leetcode-cn.com/problems/3sum/)

## 题目描述\(中等\)

Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

**Note**:

> The solution set must not contain duplicate triplets.

Example:

```
Given array nums = [-1, 0, 1, 2, -1, -4],
A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

## 思路

1. 三个值的取值遍历求和，判断，利用set去除重复的list
2. 数组排序，对选定的一个值，选取low、high两指针，判断当前三者和和目标差的正负，决定哪个指针移动，利用set去除重复的list
3. 数组排序，对选定的一个值，选取low、high两指针，判断当前三者和和目标差的正负，决定哪个指针移动，若找到目标后，双指针判断后续元素是否一致进行去重操作

## 解决方法

### 暴力遍历

三层循环，遍历所有情况，利用set把重复情况去除
```java
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> tuples = new ArrayList<>();
        Set<List<Integer>> set = new HashSet<>();
        int len = nums.length;
        if(len<3)
            return tuples;
        for(int i=0;i<len-2;i++) {
            for(int j=i+1;j<len-1;j++) {
                for(int k=j+1;k<len;k++) {
                    if(nums[i]+nums[j]+nums[k]==0) {
                        List<Integer> tuple = new ArrayList<>();
                        tuple.add(nums[i]);
                        tuple.add(nums[j]);
                        tuple.add(nums[k]);
                        set.add(tuple);
                    }
                }
            }
        }
        for (List<Integer> list : set) {
            tuples.add(list);
        }
        return tuples;
    }
```

### 双指针-排序 set去重

遍历数组，用 0 减去当前的数，作为 sum ，然后再找两个数使得和为 sum。
用两个指针，一个指向头，一个指向尾，去找这两个数字，这样的话，找另外两个数时间复杂度就会从 O(n²)，降到 O(n)。

```java
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> tuples = new ArrayList<>();
        Set<List<Integer>> set = new HashSet<>();
        Arrays.sort(nums);
        for(int i=0;i<nums.length-2;i++) {
            if(i>0&&nums[i]==nums[i-1]) {
                continue;
            }
            int low = i+1;
            int high = nums.length-1;
            while(low<high) {
                int sum = nums[low]+nums[high];
                if(sum==-nums[i]) {
                    List<Integer> tuple = Arrays.asList(nums[i],nums[low],nums[high]);
                    set.add(tuple);
                    low++;
                    high--;
                }
                else if (sum>-nums[i]) {
                    high--;
                }
                else if (sum<-nums[i]) {
                    low++;
                }
            }
        }
        for (List<Integer> list : set) {
            tuples.add(list);
        }
        return tuples;
    }
```

时间复杂度：O(n)，n 指的是 num

空间复杂度：O(N)，最坏情况，即 N 是指 n 个元素的排列组合个数，即 $N=C^3_n$​​ ，用来保存结果。


### 双指针-有序判断去重

```java
    public List<List<Integer>> threeSum2(int[] nums) {
        List<List<Integer>> tuples = new ArrayList<>();
        Arrays.sort(nums);
        int len = nums.length;
        for(int i=0;i<len-2;i++) {
            //为了保证不加入重复的 list,因为是有序的，所以如果和前一个元素相同，只需要继续后移就可以
            if(i>0&&nums[i]==nums[i-1]) {
                continue;
            }
            //两个指针,并且头指针从i + 1开始，防止加入重复的元素
            int low = i+1;
            int high = len-1;
            while(low<high) {
                int sum = nums[low]+nums[high];
                if(sum==-nums[i]) {
                    List<Integer> tuple = Arrays.asList(nums[i],nums[low],nums[high]);
                    tuples.add(tuple);
                    //元素相同要后移，防止加入重复的 list
                    while(low<high&&nums[low]==nums[low+1]) {
                        low++;
                    }
                    while(low<high&&nums[high]==nums[high-1]) {
                        high--;
                    }
                    low++;
                    high--;
                }
                else if (sum>-nums[i]) {
                    high--;
                }
                else if (sum<-nums[i]) {
                    low++;
                }
            }
        }
        return tuples;
    }
```

时间复杂度：O(n)，n 指的是 num

空间复杂度：O(N)，最坏情况，即 N 是指 n 个元素的排列组合个数，即 $N=C^3_n$​​ ，用来保存结果。



