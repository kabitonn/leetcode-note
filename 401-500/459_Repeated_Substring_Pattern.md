
# 459. Repeated Substring Pattern(E)

[459. 重复的子字符串](https://leetcode-cn.com/problems/repeated-substring-pattern/)

## 题目描述(中等)

给定一个非空的字符串，判断它是否可以由它的一个子串重复多次构成。给定的字符串只含有小写英文字母，并且长度不超过10000。

示例 1:
```
输入: "abab"

输出: True

解释: 可由子字符串 "ab" 重复两次构成。
```

示例 2:
```
输入: "aba"

输出: False
```

示例 3:
```
输入: "abcabcabcabc"

输出: True

解释: 可由子字符串 "abc" 重复四次构成。 (或者子字符串 "abcabc" 重复两次构成。)
```

## 思路

- 遍历所有可能长度子串
- KMP思想
- 正则匹配

## 解决方法


### 遍历所有可能长度子串

所有可能长度的子串为[1,n/2]

遍历所有可能，判断能否构成母串

```java
    public boolean repeatedSubstringPattern1(String s) {
        int n = s.length();
        for (int i = 1; i <= n / 2; i++) {
            if (n % i != 0) {
                continue;
            }
            String substr = s.substring(0, i);
            int index = i;
            while (index < n) {
                if (index == s.indexOf(substr, index)) {
                    index += i;
                } else {
                    break;
                }
            }
            if (index == n) {
                return true;
            }
        }
        return false;
    }
```


### 

1. 将原字符串给出拷贝一遍组成新字符串；
2. 掐头去尾留中间；
3. 如果还包含原字符串，则满足题意。

假设字符串`s`有`n`个子串`t`构成,则拼接后的子串为`2n*t`个,掐头去尾后为`(2n-2)个t`,如果此时的字符串至少包含一个原字符串s,则说明至少包含n个子串t,则 `2n-2 >= n,n >= 2`.则说明该字符串是周期性结构,最少由两个子串t构成.如果一个都不包含,即不包含n个子串,则说明 `2n-2 < n, n < 2`,即 n 为 1 ,也就是不符合周期性结构.


```java
    public boolean repeatedSubstringPattern(String s) {
        return (s + s).substring(1, 2 * s.length() - 1).contains(s);
    }

```

### KMP思想

利用KMP中的next数组

```java
    public boolean repeatedSubstringPattern2(String s) {
        int n = s.length();
        int[] next = new int[n + 1];
        next[0] = -1;
        int i = 0, j = -1;
        while (i < n) {
            if (j == -1 || s.charAt(i) == s.charAt(j)) {
                i++;
                j++;
                next[i] = j;
            } else {
                j = next[j];
            }
        }
        return next[n] != 0 && n % (n - next[n]) == 0;
    }
```



### 正则匹配

```java
    Pattern pattern = Pattern.compile("(\\w+)\\1+");

    public boolean repeatedSubstringPattern0(String s) {
        Matcher matcher = pattern.matcher(s);
        return matcher.matches();
        //return s.matches("(\\w+)\\1+");
    }

```