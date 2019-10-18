## [438. Find All Anagrams in a String](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

## 1. 题目描述\(简单\)

Given a string s and a non-empty string p, find all the start indices of p's anagrams in s.

Strings consists of lowercase English letters only and the length of both strings s and p will not be larger than 20,100.

The order of output does not matter.  
**  
Example 1:**

```
Input:
s: "cbaebabacd" p: "abc"

Output:
[0, 6]

Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
```

**Example 2:**

```
Input:
s: "abab" p: "ab"

Output:
[0, 1, 2]

Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".
```

## 2. 思路

## 3. 解决方法

### 3.1 遍历等长子串

取等长子串判断isAnagram

```java
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> list = new ArrayList<>();
        int lens = s.length(),lenp = p.length();
        if(lens<lenp||lenp==0) {return list;}
        for(int i=0;i<=lens-lenp;i++){
            if(isAnagram(s.substring(i, i+lenp), p)) {
                list.add(i);
            }
        }    
        return list;
    }
    public boolean isAnagram(String s, String t) {
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

### 3.2 滑动窗口遍历

```java
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> list = new ArrayList<>();
        int lens = s.length(),lenp = p.length();
        if(lens<lenp||lenp==0) {return list;}
        int[] map = new int[26];
        for( char c:p.toCharArray()) {
            map[c-'a']++;
        }
        int[] substr = new int[26];
        for(int start=0;start<lenp;start++) {
            substr[s.charAt(start)-'a']++;
        }
        if(Arrays.equals(map, substr)) {list.add(0);}
        for(int start=1;start<=lens-lenp;start++) {
            substr[s.charAt(start-1)-'a']--;
            substr[s.charAt(start+lenp-1)-'a']++;
            if(Arrays.equals(map, substr)) {list.add(start);}
        }
        return list;
    }
```

### 3.3 双指针+滑动窗户+计数法



```java
	public List<Integer> findAnagrams2(String s, String p) {
		List<Integer> list = new ArrayList<>();
		int lens = s.length(),lenp = p.length();
        if(lens<lenp||lenp==0) {return list;}
        int[] map = new int[26];
        for( char c:p.toCharArray()) {
    		map[c-'a']++;
    	}
        int left = 0, right = 0;
        int count = 0;
		while(right<lens) {
			if(map[s.charAt(right)-'a']-- > 0) {//先判断符合需求？，对应需求数目-1，符合数+1
				count++;
			}
			right++;
			//数目相等符合异位词要求
			if(count==lenp) {
				list.add(left);
			}
			//窗口达到最大移除左边界
			if(right-left==lenp) {
				if(map[s.charAt(left)-'a']++ >= 0) {//移除元素是符合需求？对应需求+1，符合数-1
					count--;
				}
				left++;
			}
		}
		return list;
	}
```



