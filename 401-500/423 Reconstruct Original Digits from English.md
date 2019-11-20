
# 423. Reconstruct Original Digits from English(M)

[423. 从英文中重建数字](https://leetcode-cn.com/problems/reconstruct-original-digits-from-english/)

## 题目描述(中等)

给定一个非空字符串，其中包含字母顺序打乱的英文单词表示的数字`0-9`。按升序输出原始的数字。

**注意**:
- 输入只包含小写英文字母。
- 输入保证合法并可以转换为原始的数字，这意味着像 "abc" 或 "zerone" 的输入是不允许的。
- 输入字符串的长度小于 50,000。

示例 1:
```
输入: "owoztneoer"

输出: "012" (zeroonetwo)
```
示例 2:
```
输入: "fviefuro"

输出: "45" (fourfive)
```
## 思路

- 哈希计数

通过字母的个数之间的关系得到每个数字的个数

## 解决方法

### 哈希表

```java
    public String originalDigits(String s) {
        int[] count = new int[26];
        for (char c : s.toCharArray()) {
            count[c - 'a']++;
        }
        //zero one two three four five six seven eight nine
        //zero two four six eight seven five three one nine
        int num[] = new int[10];
        num[0] = count['z' - 'a'];
        num[2] = count['w' - 'a'];
        num[4] = count['u' - 'a'];
        num[6] = count['x' - 'a'];
        num[8] = count['g' - 'a'];

        num[7] = count['s' - 'a'] - num[6];
        num[5] = count['v' - 'a'] - num[7];
        num[3] = count['t' - 'a'] - num[2] - num[8];
        num[1] = count['o' - 'a'] - num[0] - num[2] - num[4];
        num[9] = count['i' - 'a'] - num[6] - num[8] - num[5];

        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 10; i++) {
            for (int j = 0; j < num[i]; j++) {
                sb.append(i);
            }
        }
        return sb.toString();
    }

```

```java
    public String originalDigits1(String s) {
        // building hashmap letter -> its frequency
        char[] count = new char[26 + (int) 'a'];
        for (char letter : s.toCharArray()) {
            count[letter]++;
        }

        // building hashmap digit -> its frequency
        int[] out = new int[10];
        // letter "z" is present only in "zero"
        out[0] = count['z'];
        // letter "w" is present only in "two"
        out[2] = count['w'];
        // letter "u" is present only in "four"
        out[4] = count['u'];
        // letter "x" is present only in "six"
        out[6] = count['x'];
        // letter "g" is present only in "eight"
        out[8] = count['g'];
        // letter "h" is present only in "three" and "eight"
        out[3] = count['h'] - out[8];
        // letter "f" is present only in "five" and "four"
        out[5] = count['f'] - out[4];
        // letter "s" is present only in "seven" and "six"
        out[7] = count['s'] - out[6];
        // letter "i" is present in "nine", "five", "six", and "eight"
        out[9] = count['i'] - out[5] - out[6] - out[8];
        // letter "n" is present in "one", "nine", and "seven"
        out[1] = count['n'] - out[7] - 2 * out[9];

        // building output string
        StringBuilder output = new StringBuilder();
        for (int i = 0; i < 10; i++) {
            for (int j = 0; j < out[i]; j++) {
                output.append(i);
            }
        }
        return output.toString();
    }
```