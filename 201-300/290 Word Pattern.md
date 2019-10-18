# 290. Word Pattern
[290. Word Pattern](https://leetcode-cn.com/problems/word-pattern/)

## 题目描述(简单)

Given a pattern and a string str, find if str follows the same pattern.

Here follow means a full match, such that there is a bijection between a letter in pattern and a non-empty word in str.

Example 1:
```
Input: pattern = "abba", str = "dog cat cat dog"
Output: true
```
Example 2:
```
Input:pattern = "abba", str = "dog cat cat fish"
Output: false
```
Example 3:
```
Input: pattern = "aaaa", str = "dog cat cat dog"
Output: false
```
Example 4:
```
Input: pattern = "abba", str = "dog dog dog dog"
Output: false
```
**Notes**:
> You may assume pattern contains only lowercase letters, and str contains lowercase letters that may be separated by a single space.


## 思路
利用map存储word和pattern的一一对应关系

## 解决方法

### HashMap

利用mapS2C存储word和pattern的一一对应关系，若key存在则判断value值和当前char相等，若key不存在仍需判断需要加入的value不存在，如果将要加入的value存在即说明该char已经对应逼得key,无法实现一对一映射关系，返回false

```java
    public boolean wordPattern(String pattern, String str) {
    	Map<String, Character> map = new HashMap<>();
        String[] strs = str.split(" ");
        char[] ps = pattern.toCharArray();
        if(strs.length!=ps.length) {return false;}
        for(int i=0;i<strs.length;i++) {
        	if(!map.containsKey(strs[i])) {
        		if(map.containsValue(ps[i])) {return false;}
        		map.put(strs[i], ps[i]);
        	}
        	else {
				if(map.get(strs[i])!=ps[i]) {return false;}
			}
        }
        
        return true;
    }
```




