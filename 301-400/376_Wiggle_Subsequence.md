
# 376. Wiggle Subsequence(M)
[376. 摆动序列](https://leetcode-cn.com/problems/wiggle-subsequence/)

## 题目描述(中等)

如果连续数字之间的差严格地在正数和负数之间交替，则数字序列称为摆动序列。第一个差（如果存在的话）可能是正数或负数。少于两个元素的序列也是摆动序列。

例如， `[1,7,4,9,2,5]` 是一个摆动序列，因为差值 (6,-3,5,-7,3) 是正负交替出现的。相反, `[1,4,7,2,5]` 和 `[1,7,4,5,5]` 不是摆动序列，第一个序列是因为它的前两个差值都是正数，第二个序列是因为它的最后一个差值为零。

给定一个整数序列，返回作为摆动序列的最长子序列的长度。 通过从原始序列中删除一些（也可以不删除）元素来获得子序列，剩下的元素保持其原始顺序。

示例 1:
```
输入: [1,7,4,9,2,5]
输出: 6 
解释: 整个序列均为摆动序列。
```
示例 2:
```
输入: [1,17,5,10,13,15,10,5,16,8]
输出: 7
解释: 这个序列包含几个长度为 7 摆动序列，其中一个可为[1,17,10,13,10,16,8]。
```
示例 3:
```
输入: [1,2,3,4,5,6,7,8,9]
输出: 2
```

**进阶**:
你能否用 O(n) 时间复杂度完成此题?

## 思路

- 动态规划
- 贪心

## 解决方法

### 动态规划

up[i] 存的是目前为止最长的以第 i 个元素结尾的上升摆动序列的长度  
down[i] 记录的是目前为止最长的以第 i 个元素结尾的下降摆动序列的长度



```java
    public int wiggleMaxLength(int[] nums) {
        int n = nums.length;
        if (n <= 1) {
            return n;
        }
        int[] up = new int[n];
        int[] down = new int[n];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    up[i] = Math.max(up[i], down[j]);
                }
                if (nums[i] < nums[j]) {
                    down[i] = Math.max(down[i], up[j]);
                }
            }
            up[i]++;
            down[i]++;
        }
        int max = Math.max(up[n - 1], down[n - 1]);
        return max;
    }
```


时间复杂度： O(n)
空间复杂度： O(n)

### 动态规划 空间优化

- 上升的位置，意味着 nums[i] > nums[i - 1]
- 下降的位置，意味着 nums[i] < nums[i - 1]
- 相同的位置，意味着 nums[i] == nums[i - 1]

更新过程：
- 如果 nums[i] > nums[i-1] ，意味着这里在摆动上升，前一个数字肯定处于下降的位置。所以 up[i] = down[i-1] + 1 ， down[i]与 down[i-1]保持相同。

- 如果 nums[i] < nums[i-1]，意味着这里在摆动下降，前一个数字肯定处于下降的位置。所以 down[i] = up[i-1] + 1 ， up[i] 与 up[i-1]保持不变。

- 如果 nums[i] == nums[i-1]，意味着这个元素不会改变任何东西因为它没有摆动。所以 down[i]与 up[i] 与 down[i-1] 和 up[i-1] 都分别保持不变。



```java
    public int wiggleMaxLength1(int[] nums) {
        int n = nums.length;
        if (n <= 1) {
            return n;
        }
        int up = 1;
        int down = 1;
        for (int i = 1; i < n; i++) {
            if (nums[i] > nums[i - 1]) {
                up = down + 1;
            } else if (nums[i] < nums[i - 1]) {
                down = up + 1;
            }
        }
        return Math.max(up, down);
    }
```


时间复杂度： O(n)
空间复杂度： O(1)

### 贪心 

问题等价于找数组中交替的最大最小值。因此，如果我们选择中间的数字作为摆动序列的一部分，只会导致序列小于等于只选连续的最大最小元素

维护一个变量 prevdiff ，它的作用是只是当前数字的序列是上升还是下降的；
- 如果 prevdiff>0 ，那么表示目前是上升序列，需要找一个下降的元素，因此更新已找到的序列长度 diff(nums[i]-nums[i-1])负数；
- 如果prevdiff<0 ，就更新 diff(nums[i]-nums[i-1])为正数。


```java
    public int wiggleMaxLength2(int[] nums) {
        int n = nums.length;
        if (n <= 1) {
            return n;
        }
        int prevDiff = nums[1] - nums[0];
        int count = prevDiff != 0 ? 2 : 1;
        for (int i = 2; i < n; i++) {
            int diff = nums[i] - nums[i - 1];
            if (diff != 0 && diff * prevDiff <= 0) {
                count++;
                prevDiff = diff;
            }
        }
        return count;
    }
```

时间复杂度： O(n)
空间复杂度： O(1)