# 438. Find All Anagrams in a String(M)

[438. 找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

## 题目描述\(中等\)

给定一个字符串 `s` 和一个非空字符串 `p`，找到 s 中所有是 p 的字母异位词的子串，返回这些子串的起始索引。

字符串只包含小写英文字母，并且字符串 s 和 p 的长度都不超过 20100。

**说明：**
- 字母异位词指字母相同，但排列不同的字符串。
- 不考虑答案输出的顺序。

示例 1:
```
输入:
s: "cbaebabacd" p: "abc"

输出:
[0, 6]

解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的字母异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的字母异位词。
```

示例 2:
```
输入:
s: "abab" p: "ab"

输出:
[0, 1, 2]

解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的字母异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的字母异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的字母异位词。
```


## 思路

## 解决方法

### 遍历等长子串

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

### 滑动窗口遍历

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

### 双指针+滑动窗口+计数法



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



