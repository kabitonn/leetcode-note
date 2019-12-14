# 371. Sum of Two Integers(E)
[371. Sum of Two Integers](https://leetcode-cn.com/problems/sum-of-two-integers/)

## 题目描述\(简单\)

Calculate the sum of two integers a and b, but you are not allowed to use the operator + and -.

Example 1:

```
Input: a = 1, b = 2
Output: 3
```

Example 2:

```
Input: a = -2, b = 3
Output: 1
```

## 思路

## 解决方法

### 位运算

无进位的相加: $a \bigoplus b$  
每一位的进位: $(a \& b)$


1. a + b 的问题拆分为 (a 和 b 的无进位结果) + (a 和 b 的进位结果)
2. 无进位加法使用异或运算计算得出
3. 进位结果使用与运算和移位运算计算得出
4. 循环此过程，直到进位为 0



```java
    public int getSum(int a, int b) {
        while(b != 0){
            int sum = a ^ b;    //按位加但不进位
            int carry = (a & b) << 1;   //按位与表示进位
            a = sum;
            b = carry;
        }
        return a;
    }
```




