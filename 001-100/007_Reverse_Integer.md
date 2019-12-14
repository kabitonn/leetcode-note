# 007. Reverse Integer(E)
[007. Reverse Integer](https://leetcode-cn.com/problems/reverse-integer/)

## 题目描述(简单)

Given a 32-bit signed integer, reverse digits of an integer.

Example 1:

```
Input: 123
Output: 321
```

Example 2:

```
Input: -123
Output: -321
```

Example 3:

```
Input: 120
Output: 21
```

**Note:**

> Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: $[-2^{31} , 2^{31}-1]$. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

## 思路

取余得到个位数，然后除以 10 去掉个位数，变量保存当前倒置数，添加个位数字

## 解决方法

### 溢出预处理

我们对 temp = rev * 10 + pop; 进行讨论。intMAX = 2147483647 , intMin = - 2147483648 。

对于大于 intMax 的讨论，此时 x 一定是正数，pop 也是正数。
- 如果 rev > intMax / 10 ，那么没的说，此时肯定溢出了。
- 如果 rev == intMax / 10 = 2147483647 / 10 = 214748364 ，此时 rev * 10 就是 2147483640 如果 pop 大于 7 ，那么就一定溢出了。但是！如果假设 pop 等于 8，那么意味着原数 x 是 8463847412 了，输入的是 int ，而此时是溢出的状态，所以不可能输入，所以意味着 pop 不可能大于 7 ，也就意味着 rev == intMax / 10 时不会造成溢出。
- 如果 rev < intMax / 10 ，意味着 rev 最大是 214748363 ， rev * 10 就是 2147483630 , 此时再加上 pop ，一定不会溢出。

```java
    public int reverse1(int x) {
        int reverse = 0;
        while(x!=0) {
            if (reverse > Integer.MAX_VALUE/10 ) return 0;
            if (reverse < Integer.MIN_VALUE/10 ) return 0;
            reverse = reverse*10+x%10;
            x/=10;
        }
        return reverse;

    }
```

时间复杂度：$O(log_{10}(x))$  
空间复杂度：O(1)

### 数据类型预防

```java
    public int reverse(int x) {
        long reverse = 0;
        while(x!=0) {
            reverse = reverse*10+x%10;
            x/=10;
        }
        if(reverse < Integer.MIN_VALUE||reverse > Integer.MAX_VALUE) {
            return 0;
        }
        return (int)reverse;

    }
```
时间复杂度：$O(log_{10}(x))$  
空间复杂度：O(1)


