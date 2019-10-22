# 137. Single Number II(M)


[137. 只出现一次的数字 II](https://leetcode-cn.com/problems/single-number-ii/)


## 题目描述(中等)

Given a **non-empty** array of integers, every element appears three times except for one, which appears exactly once. Find that single one.

**Note**:

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

Example 1:
```
Input: [2,2,3,2]
Output: 3
```
Example 2:
```
Input: [0,1,0,1,0,1,99]
Output: 99
```


## 思路

- 排序
- 计数
- 位运算


## 解决方法



### 排序

```java
    public int singleNumber(int[] nums) {
        Arrays.sort(nums);
        for (int i = 0; i < nums.length; i++) {
            if (i + 1 < nums.length && nums[i] == nums[i + 1]) {
                while (i + 1 < nums.length && nums[i] == nums[i + 1]) {
                    i++;
                }
            } else {
                return nums[i];
            }
        }
        return 0;
    }
```

时间复杂度：O(n*log(n))。

空间复杂度：O(1)。

### 计数

```java
    public int singleNumber1(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int n : nums) {
            Integer count = map.get(n);
            count = count == null ? 1 : count + 1;
            map.put(n, count);
        }
        for (Integer n : map.keySet()) {
            if (map.get(n) == 1) {
                return n;
            }
        }
        return 0;
    }
```

时间复杂度：O(n)。

空间复杂度：O(n)。

### 位运算

```
假如例子是 1 2 6 1 1 2 2 3 3 3, 3 个 1, 3 个 2, 3 个 3,1 个 6
1     0 0 1
2     0 1 0 
6     1 1 0 
1     0 0 1
1     0 0 1
2     0 1 0
2     0 1 0
3     0 1 1  
3     0 1 1
3     0 1 1      
最右边的一列 1001100111 有 6 个 1
再往前看一列 0110011111 有 7 个 1
再往前看一列 0010000 有 1 个 1
我们只需要把是 3 的倍数的对应列写 0，不是 3 的倍数的对应列写 1    
也就是 1 1 0,也就是 6。
```

如果所有数字都出现了 3 次，那么每一列的 1 的个数就一定是 3 的倍数。之所以有的列不是 3 的倍数，就是因为只出现了 1 次的数贡献出了 1。所以所有不是 3 的倍数的列写 1，其他列写 0 ，就找到了这个出现 1 次的数。

```java
    public int singleNumber2(int[] nums) {
        int single = 0;
        //考虑每一位
        for (int i = 0; i < 32; i++) {
            int count = 0;
            //考虑每一个数
            for (int j = 0; j < nums.length; j++) {
                //当前位是否是 1
                if ((nums[j] >>> i & 1) == 1) {
                    count++;
                }
            }
            //1 的个数是否是 3 的倍数
            if (count % 3 != 0) {
                single = single | 1 << i;
            }
        }
        return single;
    }

```

时间复杂度：O(n)。

空间复杂度：O(1)。

### 位运算 通解

#### 通用方法

**问题**
给一个数组，每个元素都出现 k ( k > 1) 次，除了一个数字只出现 p 次(p >= 1, p % k !=0)，找到出现 p 次的那个数。

为了计数 k 次，我们必须要 m 个比特，其中 $$ 2^m >=k $$，也就是 $$ m >= logk $$。

假设我们 m 个比特依次是 $$ x_mx_{m-1}...x_2x_1 $$

开始全部初始化为 0。    即 00...00

然后扫描所有数字的当前 bit 位，用 i 表示当前的 bit。

```
假如例子是 1 2 6 1 1 2 2 3 3 3, 3 个 1, 3 个 2, 3 个 3,1 个 6
1     0 0 1
2     0 1 0 
6     1 1 0 
1     0 0 1
1     0 0 1
2     0 1 0
2     0 1 0
3     0 1 1  
3     0 1 1
3     0 1 1
```

初始 状态 00...00。

第一次遇到 1 , m 个比特依次是 00...01。

第二次遇到 1 , m 个比特依次是 00...10。

第三次遇到 1 , m 个比特依次是 00...11。

第四次遇到 1 , m 个比特依次是 00..100。


x1 的变化规律就是遇到 1 变成 1 ，再遇到 1 变回 0。遇到 0 的话就不变。

所以 x1 = x1 ^ i，可以用异或来求出 x1 。

x2 的话，当遇到 1 的时候，如果之前 x1 是 0，x2 就不变。如果之前 x1 是 1，对应于上边的第二次遇到 1 和第四次遇到 1。 x2 从 0 变成 1 和 从 1 变成 0。
所以 x2 的变化规律就是遇到 1 同时 x1 是 1 就变成 1，再遇到 1 同时 x1 是 1 就变回 0。遇到 0 的话就不变。和 x1 的变化规律很像，所以同样可以使用异或。

x2 = x2 ^ (i & x1)，多判断了 x1 是不是 1。

假设我们的 k = 3，那么我们应该在 10 之后就变成 00，而不是到 11。

所以我们需要一个 mask ，当没有到达 k 的时候和 mask进行与操作是它本身，当到达 k 的时候和 mask 相与就回到 00...000。

根据上边的要求构造 mask，假设 k 写成二进制以后是 $$ y_m...y_2y_1 $$。

mask = $$ ~(y_1 \& y_2 \& ... \& y_m) $$,

```java
    public int singleNumber5(int[] nums) {
        int x1 = 0, x2 = 0, mask = 0;
        for (int i : nums) {
            x2 ^= x1 & i;
            x1 ^= i;
            mask = ~(x1 & x2);
            x2 &= mask;
            x1 &= mask;
        }
        return x1;
    }
```




