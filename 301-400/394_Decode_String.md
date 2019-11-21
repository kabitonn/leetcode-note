# 394. Decode String(M)

[394. 字符串解码](https://leetcode-cn.com/problems/decode-string/)

## 题目描述(中等)

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: `k[encoded_string]`，表示其中方括号内部的 encoded_string 正好重复 k 次。注意 k 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 k ，例如不会出现像 `3a` 或 `2[4]` 的输入。

示例:
```
s = "3[a]2[bc]", 返回 "aaabcbc".
s = "3[a2[c]]", 返回 "accaccacc".
s = "2[abc]3[cd]ef", 返回 "abcabccdcdcdef".
```


## 思路

- 辅助栈
- DFS递归

## 解决方法

### 栈

- 遇到 '[' 数字入栈，空串入栈
- 遇到 ']' 数字出栈，字符串出栈，数字和字符串循环构造新的字符串
  - 若栈空，则新字符串添加进结果
  - 若栈非空，则新字符串和栈顶字符串合并
- 遇到数字
- 遇到字符
  - 若栈为空，则添加到结果中
  - 否则和栈顶字符串合并

```java
    public String decodeString(String s) {
        int[] nums = new int[s.length() / 3];
        String[] strs = new String[s.length() / 3];
        int top = 0;
        int num = 0;
        StringBuilder sb = new StringBuilder();
        for (char c : s.toCharArray()) {
            if (c >= '0' && c <= '9') {
                num = num * 10 + c - '0';
            } else if (c == '[') {
                strs[top] = "";
                nums[top] = num;
                top++;
                num = 0;
            } else if (c == ']') {
                top--;
                int k = nums[top];
                String str = strs[top];
                String tmp = str;
                for (int i = 1; i < k; i++) {
                    str += tmp;
                }
                if (top == 0) {
                    sb.append(str);
                } else {
                    strs[top - 1] = strs[top - 1] + str;
                }
            } else {
                if (top == 0) {
                    sb.append(c);
                } else {
                    strs[top - 1] = strs[top - 1] + c;
                }
            }
        }
        return sb.toString();
    }

```

### 辅助栈

- '['  当前存储字符串入栈，当前数字入栈
- ']'  字符串，数字出栈和当前字符串构造新的字符串，然后和栈顶字符串合并，作为当前存储的字符串
- 数字 
- 字符 和当前存储字符串合并

```java
    public String decodeString1(String s) {
        int[] nums = new int[s.length() / 3];
        String[] strs = new String[s.length() / 3];
        int top = 0;
        int num = 0;
        StringBuilder sb = new StringBuilder();
        for (char c : s.toCharArray()) {
            if (c >= '0' && c <= '9') {
                num = num * 10 + c - '0';
            } else if (c == '[') {
                nums[top] = num;
                strs[top] = sb.toString();
                top++;
                num = 0;
                sb = new StringBuilder();
            } else if (c == ']') {
                top--;
                int k = nums[top];
                String str = strs[top];
                String tmp = sb.toString();
                for (int i = 0; i < k; i++) {
                    str += tmp;
                }
                sb = new StringBuilder(str);
            } else {
                sb.append(c);
            }
        }
        return sb.toString();
    }
```

### 递归DFS

过程与方法1相似，'['作为一个新的开始

```java
    public String decodeString2(String s) {
        return dfs(s.toCharArray());
    }

    int i = 0;

    public String dfs(char[] s) {
        StringBuilder sb = new StringBuilder();
        int num = 0;
        while (i < s.length) {
            if (s[i] >= '0' && s[i] <= '9') {
                num = num * 10 + s[i++] - '0';
            } else if (s[i] == '[') {
                i++;
                String str = dfs(s);
                for (int k = 0; k < num; k++) {
                    sb.append(str);
                }
                num = 0;
            } else if (s[i] == ']') {
                i++;
                return sb.toString();
            } else {
                sb.append(s[i++]);
            }
        }
        return sb.toString();
    }
```