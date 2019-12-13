
# 467. Unique Substrings in Wraparound String(M)

[467. 环绕字符串中唯一的子字符串](https://leetcode-cn.com/problems/unique-substrings-in-wraparound-string/)

## 题目描述(中等)

把字符串 s 看作是`"abcdefghijklmnopqrstuvwxyz"`的无限环绕字符串，所以 s 看起来是这样的：`"...zabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcd...."`. 

现在我们有了另一个字符串 p 。你需要的是找出 s 中有多少个唯一的 p 的非空子串，尤其是当你的输入是字符串 p ，你需要输出字符串 s 中 p 的不同的非空子串的数目。 

注意: p 仅由小写的英文字母组成，p 的大小可能超过 10000。

 
示例 1:
```
输入: "a"
输出: 1
解释: 字符串 S 中只有一个"a"子字符。
```

示例 2:
```
输入: "cac"
输出: 2
解释: 字符串 S 中的字符串“cac”只有两个子串“a”、“c”。.
```

示例 3:
```
输入: "zab"
输出: 6
解释: 在字符串 S 中有六个子串“z”、“a”、“b”、“za”、“ab”、“zab”。.
```

## 思路

- 暴力遍历
- 动态规划 哈希去重
- 动态规划 计数去重

## 解决方法

### 暴力遍历

暴力遍历所有子串是否符合

!> 超时

```java
    public int findSubstringInWraproundString(String p) {
        Set<String> set = new HashSet<>();
        for (int i = 0; i < p.length(); i++) {
            for (int j = i + 1; j <= p.length(); j++) {
                String str = p.substring(i, j);
                if (containsString(str)) {
                    set.add(str);
                }
            }
        }
        return set.size();
    }

    public boolean containsString(String t) {
        if (t.length() == 1) {
            return true;
        }
        for (int i = 1; i < t.length(); i++) {
            if (t.charAt(i) != t.charAt(i - 1) + 1 && t.charAt(i - 1) - t.charAt(i) != 25) {
                return false;
            }
        }
        return true;
    }

```

### 动态规划 哈希去重

!> 超时

dp[i] 以第个字符为结束的符合的子串个数

```java
    public int findSubstringInWraproundString1(String p) {
        Set<String> set = new HashSet<>();
        int[] dp = new int[p.length()];
        for (int i = 0; i < p.length(); i++) {
            dp[i] = 1;
            if (i > 0 && isAdjacent(p.charAt(i), p.charAt(i - 1))) {
                dp[i] += dp[i - 1];
            }
            for (int j = 0; j < dp[i]; j++) {
                set.add(p.substring(i - j, i + 1));
            }
        }
        return set.size();
    }

    public boolean isAdjacent(char a, char b) {
        return a - b == 1 || b - a == 25;
    }
```
### 动态规划 计数去重

唯一的子字符串的数量就等于：分别以a, b, c, d,,,z结尾的最长子字符串的长度之后；注意， 不能统计重复的， 那么， 以z为例， 以z结尾的最长子字符串的长度， 就是， 以z结尾的“唯一”的子字符串的数量；

统计以每个字符作为结尾的最长连续序列(可以覆盖掉重复的短序列的情况), 他们的和即为所求

- dp 以当前字符为结束的符合的子串个数
- count[] 以每个字符作为结尾的最长连续序列


```java
    public int findSubstringInWraproundString2(String p) {
        char[] chars = p.toCharArray();
        int[] count = new int[26];
        int dp = 0;
        for (int i = 0; i < chars.length; i++) {
            if (i > 0 && (chars[i] - chars[i - 1] == 1 || chars[i - 1] - chars[i] == 25)) {
                dp++;
            } else {
                dp = 1;
            }
            count[chars[i] - 'a'] = Math.max(count[chars[i] - 'a'], dp);
        }
        int sum = 0;
        for (int num : count) {
            sum += num;
        }
        return sum;
        //return Arrays.stream(count).sum();
    }
```