# 153. Find Minimum in Rotated Sorted Array(M)


[]()


## 题目描述(中等)



## 思路

- 遍历
- 二分

## 解决方法



### 遍历


```java
    public int findMin(int[] nums) {
        int bias = getBiasByMax(nums);
        return nums[bias];
    }
```


### 二分搜索

```
    public int getBiasByMin(int[] nums) {
        int left = 0, right = nums.length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > nums[right]) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        return left;
    }

```

