
# 537. Complex Number Multiplication(M)

[537. 复数乘法](https://leetcode-cn.com/problems/complex-number-multiplication/)

## 题目描述(中等)

给定两个表示复数的字符串。

返回表示它们乘积的字符串。注意，根据定义 i2 = -1 。

示例 1:
```
输入: "1+1i", "1+1i"
输出: "0+2i"
解释: (1 + i) * (1 + i) = 1 + i2 + 2 * i = 2i ，你需要将它转换为 0+2i 的形式。
```

示例 2:
```
输入: "1+-1i", "1+-1i"
输出: "0+-2i"
解释: (1 - i) * (1 - i) = 1 + i2 - 2 * i = -2i ，你需要将它转换为 0+-2i 的形式。
```

**注意**:

- 输入字符串不包含额外的空格。
- 输入字符串将以 a+bi 的形式给出，其中整数 a 和 b 的范围均在 [-100, 100] 之间。输出也应当符合这种形式。



## 思路

## 解决方法

### 

两个复数的乘法可以依下述方法完成：

$$(a+ib) \times (x+iy)=ax+i^2by+i(bx+ay)=ax-by+i(bx+ay)$$


```java
    public String complexNumberMultiply(String a, String b) {
        int pos1 = a.indexOf('+');
        int pos2 = b.indexOf('+');
        int a1 = Integer.valueOf(a.substring(0, pos1));
        int b1 = Integer.valueOf(a.substring(pos1 + 1, a.length() - 1));
        int a2 = Integer.valueOf(b.substring(0, pos2));
        int b2 = Integer.valueOf(b.substring(pos2 + 1, b.length() - 1));
        int A = a1 * a2 - b1 * b2;
        int B = a1 * b2 + a2 * b1;
        return A + "+" + B + "i";
    }
```