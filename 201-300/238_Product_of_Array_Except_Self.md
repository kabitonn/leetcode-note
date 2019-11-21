# 238. Product of Array Except Self(M)

[238. 除自身以外数组的乘积](https://leetcode-cn.com/problems/product-of-array-except-self/)

## 题目描述(中等)

Given an array nums of n integers where n > 1,  return an array output such that output[i] is equal to the product of all the elements of nums except nums[i].

Example:
```
Input:  [1,2,3,4]
Output: [24,12,8,6]
```
**Note**: Please solve it **without division** and in O(n).

**Follow up**:
Could you solve it with constant space complexity? (The output array does not count as extra space for the purpose of space complexity analysis.)



## 思路


- 前向遍历 + 后向遍历

## 解决方法

### 前向遍历 + 后向遍历

除自身的乘积即为前项的乘积和后项乘积的乘积

```java
    public int[] productExceptSelf(int[] nums) {
        int[] output = new int[nums.length];
        int leftProduct = 1;
        for (int i = 0; i < nums.length; i++) {
            output[i] = leftProduct;
            leftProduct *= nums[i];
        }
        int rightProduct = 1;
        for (int i = nums.length - 1; i >= 0; i--) {
            output[i] *= rightProduct;
            rightProduct *= nums[i];
        }
        return output;
    }

```

一次遍历

```java
    public int[] productExceptSelf1(int[] nums) {
        int n = nums.length;
        int[] output = new int[n];
        int leftProduct = 1;
        int rightProduct = 1;
        Arrays.fill(output, 1);
        for (int i = 0; i < n; i++) {
            output[i] *= leftProduct;
            leftProduct *= nums[i];

            output[n - i - 1] *= rightProduct;
            rightProduct *= nums[n - i - 1];
        }
        return output;
    }
```

时间复杂度：O(n)
空间复杂度：O(n)