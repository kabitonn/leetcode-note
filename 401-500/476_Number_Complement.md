
# 476. Number Complement(E)

[476. 数字的补数](https://leetcode-cn.com/problems/number-complement/)

## 题目描述(中等)

给定一个正整数，输出它的补数。补数是对该数的二进制表示取反。

**注意**:
- 给定的整数保证在32位带符号整数的范围内。
- 你可以假定二进制数不包含前导零位。

示例 1:
```
输入: 5
输出: 2
解释: 5的二进制表示为101（没有前导零位），其补数为010。所以你需要输出2。
```

示例 2:
```
输入: 1
输出: 0
解释: 1的二进制表示为1（没有前导零位），其补数为0。所以你需要输出0。
```

## 思路

原码：Sign-Magnitude

反码：Ones' Complement -x = [11111...1] - x 1的补数

补码：Two's complement -x = 2^w - x "Two's"的意思就是"2^w"里的2

- 取反截取原位数
- 与原有位数的全1异或
  - 依次去掉低位的1
  - HashMap容量确定

## 解决方法

### 取反截取原位数

```java
    public int findComplement(int num) {
        int mask = 0;
        int n = num;
        do {
            n >>= 1;
            mask = mask << 1 | 1;
        } while (n != 0);
        n = ~num;
        return n & mask;
    }
```

### 与原有位数的全1异或

```java
    public int findComplement1(int num) {
        int mask = 1;
        while ((num & mask) != num) {
            mask = mask << 1 | 1;
        }
        return num ^ mask;
    }

    //与全1的数进行异或之后，实现按位取反
    public int findComplement2(int num) {
        int mask = 1;
        int n = num;
        do {
            n >>= 1;
            mask <<= 1;
        } while (n != 0);
        mask--;
        return num ^ mask;
    }
```


```java
    //仅符合正整数计算要求，不满足0
    public int findComplement3(int num) {
        int mask = num;
        // 不断将低位1置0，留下最高位为1的mask
        while ((mask & (mask - 1)) != 0) {
            mask &= mask - 1;
        }
        mask = (mask << 1) - 1;
        return num ^ mask;
    }
```

### HashMap容量确定原理

```java
    // HashMap的tableSizeFor求掩码,右移x位或即把1右边第x位置1
    public int findComplement4(int num) {
        int sum = num;
        sum |= sum >> 1;
        sum |= sum >> 2;
        sum |= sum >> 4;
        sum |= sum >> 8;
        sum |= sum >> 16;
        return num ^ sum;
    }
```