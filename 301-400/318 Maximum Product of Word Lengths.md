# 318. Maximum Product of Word Lengths(M)

[318. 最大单词长度乘积](https://leetcode-cn.com/problems/maximum-product-of-word-lengths/)

## 题目描述(中等)

给定一个字符串数组 `words`，找到 `length(word[i]) * length(word[j])` 的最大值，并且这两个单词不含有公共字母。你可以认为每个单词只包含小写字母。如果不存在这样的两个单词，返回 0。

示例 1:
```
输入: ["abcw","baz","foo","bar","xtfn","abcdef"]
输出: 16 
解释: 这两个单词为 "abcw", "xtfn"。
```
示例 2:
```
输入: ["a","ab","abc","d","cd","bcd","abcd"]
输出: 4 
解释: 这两个单词为 "ab", "cd"。
```
示例 3:
```
输入: ["a","aa","aaa","aaaa"]
输出: 0 
解释: 不存在这样的两个单词。
```


## 思路

- 素数乘积
- 哈希集
- 位运算表示

## 解决方法

### 素数key乘积

对于每个字符串求出其对应的素数乘积key,若有相同字母代表最大公因数不为1

字符串过长时，会溢出导致错误

```java
    public int maxProduct(String[] words) {
        int n = words.length;
        int[] prime = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101, 103};
        long[] keys = new long[n];
        for (int i = 0; i < n; i++) {
            long key = 1;
            for (char c : words[i].toCharArray()) {
                key *= prime[c - 'a'];
            }
            keys[i] = key;
        }
        int maxProduct = 0;
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                if (gcd(keys[i], keys[j]) != 1) {
                    continue;
                }
                maxProduct = Math.max(words[i].length() * words[j].length(), maxProduct);
                System.out.println(words[i] + words[i].length() + "   " + words[j] + words[j].length());
            }
        }
        return maxProduct;
    }

    public long gcd(long a, long b) {
        long r = 0;
        while (b != 0) {
            r = a % b;
            a = b;
            b = r;
        }
        return a;
    }
```


### 哈希集

集合判断是否有重复字符

```java
    public int maxProduct1(String[] words) {
        int n = words.length;
        Set<Character>[] set = new Set[n];
        for (int i = 0; i < n; i++) {
            set[i] = new HashSet<>();
            for (char c : words[i].toCharArray()) {
                set[i].add(c);
            }
        }
        int maxProduct = 0;
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                if (!both(set[i], set[j])) {
                    maxProduct = Math.max(words[i].length() * words[j].length(), maxProduct);
                }
            }
        }
        return maxProduct;
    }

    public boolean both(Set<Character> set1, Set<Character> set2) {
        if (set1.size() > set2.size()) {
            Set tmp = set1;
            set1 = set2;
            set2 = tmp;
        }
        for (char c : set1) {
            if (set2.contains(c)) {
                return true;
            }
        }
        return false;
    }
```


### 位运算

key上的每一位表示其对应的字符是否存在

两字符串没有相同字符，可以得到两个key相与为0

```java
    public int maxProduct2(String[] words) {
        int n = words.length;
        int[] keys = new int[n];
        for (int i = 0; i < n; i++) {
            for (char c : words[i].toCharArray()) {
                keys[i] |= 1 << c - 'a';
            }
        }
        int maxProduct = 0;
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                if ((keys[i] & keys[j]) == 0) {
                    maxProduct = Math.max(words[i].length() * words[j].length(), maxProduct);
                }
            }
        }
        return maxProduct;
    }

```
