# 409. Longest Palindrome(E)
[409. Longest Palindrome](https://leetcode-cn.com/problems/longest-palindrome/)

## 题目描述(简单)

Given a string which consists of lowercase or uppercase letters, find the length of the longest palindromes that can be built with those letters.

This is case sensitive, for example "Aa" is not considered a palindrome here.

**Note**:
Assume the length of given string will not exceed 1,010.

Example:
```
Input:
"abccccdd"

Output:
7

Explanation:
One longest palindrome that can be built is "dccaccd", whose length is 7.
```

## 思路

贪心法
计算每种字符出现频率，奇数的除去偶数部分只计一次

## 解决方法

### HashMap


```java
    public int longestPalindrome(String s) {
    	Map<Character, Integer> map = new HashMap<>();
    	for(char c:s.toCharArray()) {
    		int count = map.getOrDefault(c, 0);
    		map.put(c, count+1);
    	}
    	int len = 0;
    	int odd = 0;
    	for(Character c:map.keySet()) {
    		int count = map.get(c);
    		if(count%2==0) {	len+=count;}
    		else {				
    			odd = 1;
    			len+=count-1;}
    	}
    	return len+odd;
    }
```



### 数组模拟map


count/2*2 实现只计偶数部分



```java
    public int longestPalindrome1(String s) {
    	int[] map = new int[52];
    	for(char c:s.toCharArray()) {
    		if(c>='a') {map[c-'a'+26]++;}
    		else {		map[c-'A']++;}
    	}
    	int len = 0;
    	for(int count:map) {
    		len += count/2*2;
    	}
    	if(len<s.length()) {len++;}
    	return len;
    }
```


