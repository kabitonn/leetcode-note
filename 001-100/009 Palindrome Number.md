# 009. Palindrome Number
[009. Palindrome Number](https://leetcode-cn.com/problems/palindrome-number/)

## 1. 题目描述\(简单\)

Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.

Example 1:

```
Input: 121
Output: true
```

Example 2:

```
Input: -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. 
             Therefore it is not a palindrome.
```

Example 3:

```
Input: 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
```

Follow up:

> Coud you solve it without converting the integer to a string?

## 2. 思路

Number倒置判断相等？

## 3. 解决方法

### 3.1 完全reverse

```java
    public boolean isPalindrome(int x) {
        if(x<0) {
            return false;
        }
        int y=0;
        int tmp = x;
        while(x!=0) {
            y=y*10+x%10;
            x/=10;
        }
        return tmp==y?true:false;
    }
```

时间复杂度：和求转置一样，x 有多少位，就循环多少次，所以是 $$O(log_{10}(x))$$

空间复杂度：O(1)

### 3.2 一半reverse

```java
    public boolean isPalindrome(int x) {
        if(x<0) {
            return false;
        }
        int digit = (int)Math.log10(x)+1;
        int y = 0;
        for(int i=0;i<digit/2;i++) {
            y=y*10+x%10;
            x/=10;
        }
        if(digit%2==0) {
            return x==y?true:false;
        }
        else {
            return (x/10)==y?true:false;
        }
    }
```

时间复杂度：循环 x 的总位数的一半次，所以时间复杂度依旧是 $$O(log_{10}(x))$$。

空间复杂度：O(1)常数个变量。

