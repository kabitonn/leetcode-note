
# 481. Magical String(M)

[481. 神奇字符串](https://leetcode-cn.com/problems/magical-string/)

## 题目描述(中等)

神奇的字符串 S 只包含 '1' 和 '2'，并遵守以下规则：

字符串 S 是神奇的，因为串联字符 '1' 和 '2' 的连续出现次数会生成字符串 S 本身。

字符串 S 的前几个元素如下：S = “1221121221221121122 ......”

如果我们将 S 中连续的 1 和 2 进行分组，它将变成：

1 22 11 2 1 22 1 22 11 2 11 22 ......

并且每个组中 '1' 或 '2' 的出现次数分别是：

1 2 2 1 1 2 1 2 2 1 2 2 ......

你可以看到上面的出现次数就是 S 本身。

给定一个整数 N 作为输入，返回神奇字符串 S 中前 N 个数字中的 '1' 的数目。

**注意**：N 不会超过 100,000。

示例：
```
输入：6
输出：3
解释：神奇字符串 S 的前 6 个元素是 “12211”，它包含三个 1，因此返回 3。
```

## 思路

- 题意生成字符串，统计个数

## 解决方法

### 生成字符串 统计个数

固定前三位，后续题意规律生成

```java
    public int magicalString(int n) {
        if (n <= 0) {
            return 0;
        }
        if (n < 4) {
            return 1;
        }
        char[] chars = new char[n];
        int count = 1;
        chars[0] = '1';
        chars[1] = '2';
        chars[2] = '2';
        int i = 3, j = 2;
        while (i < n) {
            if (chars[j] == '1') {
                if (chars[i - 1] == '1') {
                    chars[i++] = '2';
                } else {
                    chars[i++] = '1';
                    count++;
                }
            } else if (chars[j] == '2') {
                if (chars[i - 1] == '1') {
                    chars[i++] = '2';
                    if (i < n) {
                        chars[i++] = '2';
                    }
                } else {
                    chars[i++] = '1';
                    count++;
                    if (i < n) {
                        chars[i++] = '1';
                        count++;
                    }
                }
            }
            j++;
        }
        return count;
    }
```

### 依次延伸字符串

固定字符串的第一位，从第二位开始延伸

```java
    public int magicalString1(int n) {
        if (n <= 0) {
            return 0;
        }
        int[] chars = new int[n];
        chars[0] = 1;
        int i = 1, j = 1;
        int count = 1, num = 2, c = 2;
        while (i < n) {
            if (num > 0) {
                chars[i++] = c;
                num--;
            }
            if (c == 1) {
                count++;
            }
            if (num == 0) {
                num = chars[++j];
                c = c == 2 ? 1 : 2;
            }
        }
        return count;
    }
```