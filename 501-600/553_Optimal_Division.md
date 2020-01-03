
# 553. Optimal Division(M)
 
[553. 最优除法](https://leetcode-cn.com/problems/optimal-division/)

## 题目描述(中等)

给定一组正整数，相邻的整数之间将会进行浮点除法操作。例如， [2,3,4] -> 2 / 3 / 4 。

但是，你可以在任意位置添加任意数目的括号，来改变算数的优先级。你需要找出怎么添加括号，才能得到最大的结果，并且返回相应的字符串格式的表达式。你的表达式不应该含有冗余的括号。

示例：
```
输入: [1000,100,10,2]
输出: "1000/(100/10/2)"
解释:
1000/(100/10/2) = 1000/((100/10)/2) = 200
但是，以下加粗的括号 "1000/((100/10)/2)" 是冗余的，
因为他们并不影响操作的优先级，所以你需要返回 "1000/(100/10/2)"。

其他用例:
1000/(100/10)/2 = 50
1000/(100/(10/2)) = 50
1000/100/10/2 = 0.5
1000/100/(10/2) = 2
```
说明:

- 输入数组的长度在 [1, 10] 之间。
- 数组中每个元素的大小都在 [2, 1000] 之间。
- 每个测试用例只有一个最优除法解。

## 思路

a,b,c,d 结果最大，分子a已经确定了，所有只要分母尽量小就可以了，又因为都是大于1的整数，b/c/d肯定是最小了，以此类推

## 解决方法

### 数学

考虑输入形如 [a,b,c,d] 的列表，需要设置一些运算优先级来最大化 a/b/c/d。我们知道为了最大化 p/q，分母 q 应该最小化，所以为了最大化 a/b/c/d 我们首先需要最小化 b/c/d，现在我们的目标变成了最小化表达式 b/c/d。

显然，对于 d>1 我们有 d/c > 1/(d*c)。

```java
    public String optimalDivision(int[] nums) {
        int length = nums.length;
        if (length == 0) {
            return "";
        } else if (length == 1) {
            return nums[0] + "";
        } else if (length == 2) {
            return nums[0] + "/" + nums[1];
        }
        StringBuilder sb = new StringBuilder();
        sb.append(nums[0]);
        sb.append("/(");
        for (int i = 1; i < length - 1; i++) {
            sb.append(nums[i]);
            sb.append("/");
        }
        sb.append(nums[length - 1]);
        sb.append(")");
        return sb.toString();
    }
```