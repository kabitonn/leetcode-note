
# 482. License Key Formatting(E)

[482. 密钥格式化](https://leetcode-cn.com/problems/license-key-formatting/)

## 题目描述(中等)

给定一个密钥字符串S，只包含字母，数字以及 '-'（破折号）。N 个 '-' 将字符串分成了 N+1 组。给定一个数字 K，重新格式化字符串，除了第一个分组以外，每个分组要包含 K 个字符，第一个分组至少要包含 1 个字符。两个分组之间用 '-'（破折号）隔开，并且将所有的小写字母转换为大写字母。

给定非空字符串 S 和数字 K，按照上面描述的规则进行格式化。

示例 1：
```
输入：S = "5F3Z-2e-9-w", K = 4

输出："5F3Z-2E9W"

解释：字符串 S 被分成了两个部分，每部分 4 个字符；
     注意，两个额外的破折号需要删掉。
```

示例 2：
```
输入：S = "2-5g-3-J", K = 2

输出："2-5G-3J"

解释：字符串 S 被分成了 3 个部分，按照前面的规则描述，第一部分的字符可以少于给定的数量，其余部分皆为 2 个字符。
```

**提示**:
- S 的长度不超过 12,000，K 为正整数
- S 只包含字母数字（a-z，A-Z，0-9）以及破折号'-'
- S 非空


## 思路

倒序遍历，统计分组内个数并添加 '-' 

## 解决方法

### 倒序遍历 直接构造

分组个数满足时添加 '-'

```java
    public String licenseKeyFormatting(String S, int K) {
        char[] chars = S.toCharArray();
        StringBuilder sb = new StringBuilder();
        int len = 0;
        for (int i = chars.length - 1; i >= 0; i--) {
            if (chars[i] == '-') {
                continue;
            }
            if (len % K == 0 && len != 0) {
                sb.append('-');
            }
            if (chars[i] >= 'a' && chars[i] <= 'z') {
                chars[i] -= 32;
            }
            sb.append(chars[i]);
            len++;
        }
        return sb.reverse().toString();
    }
```

### 倒序遍历 统计长度

提前统计分组个数，也就是 '-' 的添加个数

```java
    public String licenseKeyFormatting1(String S, int K) {
        int count = 0;
        char[] chars = S.toCharArray();
        for (char ch : chars) {
            if (ch == '-') {
                count++;
            }
        }
        int len = chars.length - count;
        if (len == 0) {
            return "";
        }
        int num = len % K == 0 ? len / K - 1 : len / K;
        len += num;
        char[] str = new char[len];
        count = 0;
        int pos = len - 1;
        for (int i = chars.length - 1; i >= 0; i--) {
            if (chars[i] == '-') {
                continue;
            }
            if (count % K == 0 && count != 0) {
                str[pos--] = '-';
            }
            if (chars[i] >= 'a' && chars[i] <= 'z') {
                chars[i] -= 32;
            }
            str[pos--] = chars[i];
            count++;
        }
        return new String(str);
    }
```

### 提前分配较大长度

提前分配length*2的内存，pos记录字符串起始位置

```java
    public String licenseKeyFormatting2(String S, int K) {
        int count = 0;
        char[] chars = S.toCharArray();
        int len = chars.length * 2;
        char[] str = new char[len];
        int pos = len - 1;
        for (int i = chars.length - 1; i >= 0; i--) {
            if (chars[i] == '-') {
                continue;
            }
            if (count % K == 0 && count != 0) {
                str[pos--] = '-';
            }
            if (chars[i] >= 'a' && chars[i] <= 'z') {
                chars[i] -= 32;
            }
            str[pos--] = chars[i];
            count++;
        }
        return new String(str, ++pos, len - pos);
    }
```