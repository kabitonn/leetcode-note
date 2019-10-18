# 400. Nth Digit
[400. Nth Digit](https://leetcode-cn.com/problems/nth-digit/)

## 题目描述\(简单\)

Find the $$n^{th}$$ digit of the infinite integer sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ...

Note:  
n is positive and will fit within the range of a 32-bit signed integer $$(n < 2^{31})$$.

Example 1:

```
Input:
3

Output:
3
```

Example 2:

```
Input:
11

Output:
0

Explanation:
The 11th digit of the sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ... is a 0, which is part of the number 10.
```

## 思路

```
1-9/10-99/100-999/……
```

每组个数是上一组的10倍，位数比上一组多1；求出n是哪一组第几个数的哪一位

## 解决方法

### 1

num为所求位数所在的数字

```java
    public int findNthDigit(int n) {
        int num = 0;
        int bits = 1;
        int count = 9;
        while (n - count * bits >= 0 && count < Integer.MAX_VALUE / bits) {
            num += count;
            n -= count * bits;
            count *= 10;
            bits++;
        }
        num += n / bits;
        int r = n % bits;
        if (r == 0) {
            return num % 10;
        } else {
            //return (int) ((num+1)/Math.pow(10, bit-r))%10;
            return ((num + 1) + "").charAt(r - 1) - '0';
        }
    }
```

### 2

将n-1，避免了所求数字在相邻间切换

```java
    public int findNthDigit2(int n) {
        /*1-9/10-99/100-999/……*/
        int first = 1;  //first 是每组的第一个值
        int bits = 1;   //每组第一个值
        int count = 9;  //每组数字个数
        n--;    //从0开始
        while (n / bits / count >= 1) {
            n -= count * bits;
            count *= 10;
            first *= 10;
            bits++;
        }
        return (first+n/bits+"").charAt(n%bits)-'0';
    }
```



