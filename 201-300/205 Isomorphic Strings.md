# 205. Isomorphic Strings(E)
[205. Isomorphic Strings](https://leetcode-cn.com/problems/isomorphic-strings/)

## 题目描述(简单)

Given two strings s and t, determine if they are isomorphic.

Two strings are isomorphic if the characters in s can be replaced to get t.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character but a character may map to itself.

Example 1:
```
Input: s = "egg", t = "add"
Output: true
```
Example 2:
```
Input: s = "foo", t = "bar"
Output: false
```
Example 3:
```
Input: s = "paper", t = "title"
Output: true
```
**Note:**
- You may assume both s and t have the same length.


## 思路

## 解决方法

### HashMap替换对应字符
哈希映射，两个字符串相互映射(一对一)。

```java
    public boolean isIsomorphic(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        char[] ss = s.toCharArray();
        char[] tt = t.toCharArray();
        Map<Character, Character> map = new HashMap<>();
        for (int i = 0; i < ss.length; i++) {
            if (!map.containsKey(ss[i])) {
                if (map.containsValue(tt[i])) {
                    return false;
                }
                map.put(ss[i], tt[i]);
            } else {
                if (map.get(ss[i]) != tt[i]) {
                    return false;
                }
            }
        }
        return true;
    }
```


数组替代Map

```java
    public boolean isIsomorphic(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        char[] ss = s.toCharArray();
        char[] tt = t.toCharArray();
        char[] mapS2T = new char[128];
        char[] mapT = new char[128];
        for (int i = ss.length - 1; i >= 0; i--) {
            if (mapS2T[ss[i]] != mapT[tt[i]]) {
                return false;
            }
            mapS2T[ss[i]] = mapT[tt[i]] = tt[i];
        }
        return true;
    }
```



### 字符首次出现的位置比较

对比两个字符串对应位置的字符在字符串内第一次出现的位置。

```java
    public boolean isIsomorphic(String s, String t) {
    	if(s.length()!=t.length()) {return false;}
    	for(int i=0;i<s.length();i++) {
    		if(s.indexOf(s.charAt(i))!=t.indexOf(t.charAt(i))) {return false;}
    	}
    	return true;
    }
```


```java
    public boolean isIsomorphic(String s, String t) {
    	if(s.length()!=t.length()) {return false;}
    	char[] ss = s.toCharArray();
    	char[] tt = t.toCharArray();
    	int[] sIndex = new int[256];
    	int[] tIndex = new int[256];
    	for(int i=ss.length-1;i>=0;i--) {
    		if(sIndex[ss[i]]!=tIndex[tt[i]]) {return false;}
    		sIndex[ss[i]] = tIndex[tt[i]] = i;
    	}
    	return true;
    }
```





