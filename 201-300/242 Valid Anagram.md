## [242. Valid Anagram](https://leetcode-cn.com/problems/valid-anagram/)

## 1. 题目描述(简单)

Given two strings s and t , write a function to determine if t is an anagram of s.

Example 1:
```
Input: s = "anagram", t = "nagaram"
Output: true
```
Example 2:
```
Input: s = "rat", t = "car"
Output: false
Note:
You may assume the string contains only lowercase alphabets.
```
**Follow up:**
> What if the inputs contain unicode characters? How would you adapt your solution to such case?


## 2. 思路

1. 判断每种字符个数相等
2. 排序

## 3. 解决方法

### 3.1 哈希表


```java
    public boolean isAnagram(String s, String t) {
    	if(s.length()!=t.length()) {return false;}
    	Map<Character, Integer> map = new HashMap<>();
    	char[] ss = s.toCharArray();
    	char[] tt = t.toCharArray();
    	for(char c:ss) {
    		int count = map.getOrDefault(c, 0);
    		map.put(c, count+1);
    	}
    	for( char c:tt) {
    		int count = map.getOrDefault(c, 0);
    		map.put(c, count-1);
    	}
    	for(char c:map.keySet()) {
    		if(map.get(c)!=0) {
    			return false;
    		}
    	}
        return true;
    }
```
时间复杂度：$$O(n)$$。因为访问计数器表是一个固定的时间操作。
空间复杂度：$$O(1)$$。





### 3.2 数组模拟哈希


```java
    public boolean isAnagram(String s, String t) {
    	if(s.length()!=t.length()) {return false;}
    	int[] map = new int[26];
    	char[] ss = s.toCharArray();
    	char[] tt = t.toCharArray();
    	for(char c:ss) {
    		int index = c-'a';
    		map[index]++;
    	}
    	for( char c:tt) {
    		int index = c-'a';
    		map[index]--;
    		if(map[index]<0){return false;}
    	}
    	return true;
    }
```


```java
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        int[] counter = new int[26];
        for (int i = 0; i < s.length(); i++) {
            counter[s.charAt(i) - 'a']++;
            counter[t.charAt(i) - 'a']--;
        }
        for (int count : counter) {
            if (count != 0) {
                return false;
            }
        }
        return true;
    }
```




时间复杂度：$$O(n)$$。因为访问计数器表是一个固定的时间操作。
空间复杂度：$$O(1)$$。

### 3.3 排序


```java
    public boolean isAnagram3(String s, String t) {
    	char[] ss = s.toCharArray();
    	char[] tt = t.toCharArray();
    	Arrays.sort(ss);
    	Arrays.sort(tt);
    	return Arrays.equals(ss, tt);
    }
```
时间复杂度：$$O(n \log n)$$，假设 n 是 s 的长度，排序成本 $$O(n\log n)$$ 和比较两个字符串的成本 $$O(n)$$。排序时间占主导地位，总体时间复杂度为 $$O(n \log n)$$。
空间复杂度：$$O(1)$$，空间取决于排序实现，


