# 187. Repeated DNA Sequences(M)


[187. 重复的DNA序列](https://leetcode-cn.com/problems/repeated-dna-sequences/)


## 题目描述(中等)

All DNA is composed of a series of nucleotides abbreviated as A, C, G, and T, for example: "ACGAATTCCG". When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.

Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.

Example:
```
Input: s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"

Output: ["AAAAACCCCC", "CCCCCAAAAA"]
```

## 思路



## 解决方法



### 暴力遍历

双重循环遍历所有情况，set判断是否重复

```java
    public List<String> findRepeatedDnaSequences(String s) {
        List<String> list = new ArrayList<>();
        Set<String> set = new HashSet<>();
        for (int i = 0; i <= s.length() - 10; i++) {
            String sub = s.substring(i, i + 10);
            if (set.contains(sub)) {
                continue;
            }
            boolean repeat = false;
            for (int j = i + 1; j <= s.length() - 10; j++) {
                if (sub.equals(s.substring(j, j + 10))) {
                    repeat = true;
                    break;
                }
            }
            if (repeat && !set.contains(sub)) {
                list.add(sub);
                set.add(sub);
            }
        }
        return list;
    }

```

### 哈希表

顺序遍历DNA子串，存入HashMap，若已存在代表重复，并且未添加入结果集时添加

```java
    public List<String> findRepeatedDnaSequences1(String s) {
        List<String> list = new ArrayList<>();
        Map<String, Boolean> map = new HashMap<>();
        for (int i = 0; i <= s.length() - 10; i++) {
            String sub = s.substring(i, i + 10);
            if (!map.containsKey(sub)) {
                map.put(sub, false);
            } else if (map.containsKey(sub) && !map.get(sub)) {
                map.put(sub, true);
                list.add(sub);
            }
        }
        return list;
    }

```

### 两个哈希集

两个set,一个存放遍历的结果，一个存放重复子串，结果为重复子串

```java
    public List<String> findRepeatedDnaSequences2(String s) {
        Set<String> set1 = new HashSet<>(), set2 = new HashSet<>();
        for (int i = 0; i <= s.length() - 10; i++) {
            String sub = s.substring(i, i + 10);
            if (set1.contains(sub)) {
                set2.add(sub);
            } else {
                set1.add(sub);
            }
        }
        return new ArrayList<>(set2);
    }
```

### 

字符串中只存在四个字符 A，C，G ，T,那么可以用两位的二进制（00，01，10，11）来表示这四个字符，
那么十个字符就对应二十位的二进制，可以转换成对应的二进制表示，

又由于每个字符有四种可能，那么就定义1<<20大小的nums数组来表示所有的字符是出现的次数，


1.定义k表示二进制18位都为1的数字，与转换的数字key与运算，可以保留对应数字的后十八位，
也就是对应9个字符，然后再加上下一个字符，就是对应的十个字符的二进制数字，

2.然后根据nums[key]来判断出现次数
- 若为1，则添加到结果
3. nums[key]++


```java
    //位运算 数组标记
    public List<String> findRepeatedDnaSequences3(String s) {
        List<String> list = new ArrayList<>();
        int[] nums = new int[1 << 20];
        int mask = (1 << 18) - 1;
        int key = 0;
        for (int i = 0; i < s.length(); i++) {
            key <<= 2;
            key += getValue(s.charAt(i));
            if (i >= 9) {
                if (nums[key] == 1) {
                    list.add(s.substring(i - 9, i + 1));
                }
                nums[key]++;
                key &= mask;
            }
        }
        return list;
    }

    private int getValue(char c) {
        switch (c) {
            case 'A':
                return 0;
            case 'C':
                return 1;
            case 'G':
                return 2;
            case 'T':
                return 3;
            default:
                throw new IllegalArgumentException("Illegal character");
        }
    }
```


