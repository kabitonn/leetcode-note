# 344. Reverse String
[344. Reverse String](https://leetcode-cn.com/problems/reverse-string/)

## 题目描述(简单)

Write a function that reverses a string. The input string is given as an array of characters char[].

Do not allocate extra space for another array, you must do this by **modifying the input array** in-place with O(1) extra memory.

You may assume all the characters consist of printable ascii characters.

 

Example 1:
```
Input: ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]
```
Example 2:
```
Input: ["H","a","n","n","a","h"]
Output: ["h","a","n","n","a","H"]
```

## 思路

## 解决方法

### 双指针 交换位置


```java
    public void reverseString(char[] s) {
        int i = 0, j = s.length - 1;
        while(i < j) {
            char tmp = s[i];
            s[i++] = s[j];
            s[j--] = tmp;
        }
    }
```



### 双指针 异或交换


```java
    public void reverseString(char[] s) {
        int i = 0, j = s.length - 1;
        while(i < j) {
            s[i] ^= s[j];
            s[j] ^= s[i];
            s[i] ^= s[j];
            i++;j--;
        }
    }
```






