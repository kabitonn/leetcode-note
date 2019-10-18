# 387. First Unique Character in a String(E)
[387. First Unique Character in a String](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)

## 题目描述(简单)

Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.

Examples:
```
s = "leetcode"
return 0.

s = "loveleetcode",
return 2.
```

**Note:** You may assume the string contain only lowercase letters.


## 思路

## 解决方法

### 哈希表+遍历哈希

map存储字符出现的索引，若多次出现将其修改为-1；
```java
    public int firstUniqChar(String s) {
        Map<Character, Integer> map = new HashMap<>();
        int i = 0;
        for(char c:s.toCharArray()) {
        	if(!map.containsKey(c)) {
        		map.put(c, i);
        	}
        	else {
				map.put(c, -1);
			}
        	i++;
        }
        int first = -1;
        for(Character c:map.keySet()) {
        	int index = map.get(c);
        	if(index!=-1&&(first==-1||first>index)) {
        		first = index;
        	}
        }
        return first;
    }
```
### 哈希表+遍历字符
map存储字符出现的频率，自前向后遍历第一个1即为结果



```java
	public int firstUniqChar(String s) {
		Map<Character, Integer> map = new HashMap<>();
		for(char c:s.toCharArray()) {
			int count = map.getOrDefault(c, 0);
			map.put(c, ++count);
		}
		for(int i=0;i<s.length();i++) {
			if(map.get(s.charAt(i))==1){
				return i;
			}
		}
		return -1;
	}
```


```java
    public int firstUniqChar(String s) {
        char[] str = s.toCharArray();
        int[] map = new int[26];
        for(char c:str) {
        	map[c-'a']++;
        }
        for(int i=0;i<str.length;i++) {
        	if(map[str[i]-'a']==1) {
        		return i;
        	}
        }
        return -1;
    }
```



### 首次出现索引和末次出现索引


```java
    public int firstUniqChar2(String s) {
    	int first = -1;
    	for(char c = 'a';c<='z';c++) {
    		int firstIndex = s.indexOf(c);
    		int lastIndex = s.lastIndexOf(c);
    		if(firstIndex==-1||firstIndex!=lastIndex) {continue;}
    		if(first==-1||first>firstIndex) {
    			first = firstIndex;
    		}
    	}
    	return first;
    }
```


