# 345. Reverse Vowels of a String(E)
[345. Reverse Vowels of a String](https://leetcode-cn.com/problems/reverse-vowels-of-a-string/)

## 题目描述\(简单\)

Write a function that takes a string as input and reverse only the vowels of a string.

Example 1:

```
Input: "hello"
Output: "holle"
```

Example 2:

```
Input: "leetcode"
Output: "leotcede"
```

**Note:**

> The vowels does not include the letter "y".

## 思路

## 解决方法

### 双指针

```java
    public String reverseVowels(String s) {
        char[] str = s.toCharArray();
        int i=0,j=str.length-1;
        while(i<j) {
            if(isVowel(str[i])&&isVowel(str[j])) {
                swap(str, i++, j--);
                continue;
            }
            if(!isVowel(str[i])) {    i++;}
            if(!isVowel(str[j])) {    j--;}
        }
        return new String(str);
    }
    public boolean isVowel(char c) {
        if(c=='a'||c=='i'||c=='u'||c=='o'||c=='e') {return true;}
        if(c=='A'||c=='I'||c=='U'||c=='O'||c=='E') {return true;}
        return false;
    }
    public void swap(char[] s, int i,int j) {
        char tmp = s[i];
        s[i] = s[j];
        s[j] = tmp;
    }
```

```java
    public String reverseVowels(String s) {
        char[] str = s.toCharArray();
        int i=0,j=str.length-1;
        while(i<j) {
            while(i<j&&!isVowel(str[i])) {    i++;}
            while(i<j&&!isVowel(str[j])) {    j--;}
                swap(str, i++, j--);
        }
        return new String(str);
    }
```



