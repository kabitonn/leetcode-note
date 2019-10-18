# 017. Letter Combinations of a Phone Number(M)
[017. Letter Combinations of a Phone Number](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

## 题目描述\(中等\)

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters \(just like on the telephone buttons\) is given below. Note that 1 does not map to any letters.

![](/assets/001-100/017-problem-1.png)

Example:

```
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

**Note**:

> Although the above answer is in lexicographical order, your answer could be in any order you want.

## 思路

1. 针对当前数字对应的所有字符，添加在list已有字符串
2. 利用队列对当前前缀字符串添加当前数字对应所有字符
3. 递归：对当前前缀字符串添加当前数字对应所有字符

## 解决方法

### 迭代

```java
    public List<String> letterCombinations(String digits) {

        List<String> list = new ArrayList<>();
        if(digits.length()==0)
            return list;
        Map<Character, String> map = new HashMap<>();
        map.put('2', "abc");
        map.put('3', "def");
        map.put('4', "ghi");
        map.put('5', "jkl");
        map.put('6', "mno");
        map.put('7', "pqrs");
        map.put('8', "tuv");
        map.put('9', "wxyz");
        list.add("");
        for(char ch:digits.toCharArray()) {
            String s = map.get(ch);
            List<String> tmpList = new ArrayList<>();
            for(char c:s.toCharArray()) {
                for(String str:list) {
                    tmpList.add(str+c);
                }
            }
            list = tmpList;
        }
        return list;
    }
```

### 队列迭代

```java
    private String[] mapping = new String[] {"0", "1", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};

    public List<String> letterCombinations(String digits) {
        LinkedList<String> list = new LinkedList<>();
        if(digits.length()==0)
            return list;
        list.add("");
        while(list.peek().length()!=digits.length()) {
            String head = list.remove();
            String s = mapping[digits.charAt(head.length())-'0'];
            for( char c:s.toCharArray()) {
                list.add(head+c);
            }
        }
        return list;
    }
```

### 递归

```java
    private String[] mapping = new String[] {"0", "1", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    public List<String> letterCombinations(String digits) {
        List<String> list = new ArrayList<>();
        if(digits.equals(""))
            return list;
        combination(digits,"",list);
        return list;
    }
    public void combination(String digits, String prefix, List<String> list) {
        if(prefix.length()==digits.length()){
            list.add(prefix);
            return;
        }
        String s = mapping[digits.charAt(prefix.length())-'0'];
        for(char c:s.toCharArray()){
            combination(digits, prefix+c, list);
        }
    }
```



