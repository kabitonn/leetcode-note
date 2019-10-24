# 228. Summary Ranges(M)

[228. 汇总区间](https://leetcode-cn.com/problems/summary-ranges/)

## 题目描述(中等)

Given a sorted integer array without duplicates, return the summary of its ranges.

Example 1:
```
Input:  [0,1,2,4,5,7]
Output: ["0->2","4->5","7"]
Explanation: 0,1,2 form a continuous range; 4,5 form a continuous range.
```
Example 2:
```
Input:  [0,2,3,4,6,8,9]
Output: ["0","2->4","6","8->9"]
Explanation: 2,3,4 form a continuous range; 8,9 form a continuous range.
```

## 思路

- 遍历
- 双指针


## 解决方法

### 遍历

从前向后遍历每个点的结束点，判断结束点与开始点是否相同，不同则生成区间加入，相同则加入节点；

```java
    public List<String> summaryRanges(int[] nums) {
        List<String> list = new ArrayList<>();
        int len = nums.length;
        for (int i = 0; i < len; ) {
            int start = nums[i++];
            while (i < len && nums[i] == nums[i - 1] + 1) {
                i++;
            }
            int end = nums[i - 1];
            String s = String.valueOf(start);
            if (start != end) {
                s = s + "->" + String.valueOf(end);
            }
            list.add(s);
        }
        return list;
    }

```

时间复杂度：O(n) 每个元素都需要被访问常量次数，要么在相邻元素中被比较，要么就是被放入结果的时候被访问。

空间复杂度：O(1) 如果不考虑输出的话，我们唯一需要额外开辟空间的就是一段区间的两个坐标。


### 双指针

循环中从从当前开始点向后移动，判断结束点和开始点下标，不同则生成区间加入，相同则加入节点；
最后下一轮从结束点后一个开始

```java
public List<String> summaryRanges1(int[] nums) {
        List<String> list = new ArrayList<>();
        int len = nums.length;
        int i = 0;
        while (i < len) {
            int j = i;
            while (j < len - 1 && nums[j + 1] == nums[j] + 1) {
                j++;
            }
            String s = String.valueOf(nums[i]);
            if (i != j) {
                s = s + "->" + String.valueOf(nums[j]);
            }
            list.add(s);
            i = j + 1;
        }
        return list;
    }
```

时间复杂度：O(n) 每个元素都需要被访问常量次数，要么在相邻元素中被比较，要么就是被放入结果的时候被访问。

空间复杂度：O(1) 如果不考虑输出的话，我们唯一需要额外开辟空间的就是一段区间的两个坐标。