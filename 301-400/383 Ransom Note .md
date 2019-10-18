# 383. Ransom Note
[383. Ransom Note](https://leetcode-cn.com/problems/ransom-note/)

## 题目描述(简单)

Given an arbitrary ransom note string and another string containing letters from all the magazines, write a function that will return true if the ransom note can be constructed from the magazines ; otherwise, it will return false.

Each letter in the magazine string can only be used once in your ransom note.

**Note:**
> You may assume that both strings contain only lowercase letters.
```
canConstruct("a", "b") -> false
canConstruct("aa", "ab") -> false
canConstruct("aa", "aab") -> true
```

## 思路

## 解决方法

### HashMap
map保存每种字符的个数，ransom出现一次减一，小于0则说明无法构造

```java
    public boolean canConstruct(String ransomNote, String magazine) {
        char[] ransom = ransomNote.toCharArray();
        char[] maga = magazine.toCharArray();
        Map<Character, Integer> map = new HashMap<>();
        int count;
        for(char c:maga) {
        	count = map.getOrDefault(c, 0);
        	map.put(c, ++count);
        }
        for(char c:ransom) {
        	count = map.getOrDefault(c, 0);
        	count--;
        	if(count<0) {return false;}
        	map.put(c, count);
        }
        return true;
    }
```
```java
    public boolean canConstruct(String ransomNote, String magazine) {
        char[] ransom = ransomNote.toCharArray();
        char[] maga = magazine.toCharArray();
        int[] map = new int[26];
        for(char c:maga) {
        	map[c-'a']++;
        }
        for(char c:ransom) {
        	map[c-'a']--;
        	if(map[c-'a']<0) {return false;}
        }
        return true;
    }
```




