
# 520. Detect Capital(E)

[520. 检测大写字母](https://leetcode-cn.com/problems/detect-capital/)

## 题目描述(简单)

给定一个单词，你需要判断单词的大写使用是否正确。

我们定义，在以下情况时，单词的大写用法是正确的：

- 全部字母都是大写，比如"USA"。
- 单词中所有字母都不是大写，比如"leetcode"。
- 如果单词不只含有一个字母，只有首字母大写， 比如 "Google"。
  
否则，我们定义这个单词没有正确使用大写字母。

示例 1:
```
输入: "USA"
输出: True
```

示例 2:
```
输入: "FlaG"
输出: False
注意: 输入是由大写和小写拉丁字母组成的非空单词。
```

## 思路


## 解决方法

### 遍历

- 长度小于2的字符串一定符合规则
- 长度大于2的字符串后续字符大小写和第二的字符相同
- 前两个字符若为小写大写不符合规则

```java
        public boolean detectCapitalUse(String word) {
        if (word == null || word.length() < 2) {
            return true;
        }
        boolean first = isUpperCase(word.charAt(0));
        boolean prev = isUpperCase(word.charAt(1));
        if (!first && prev) {
            return false;
        }
        for (int i = 2; i < word.length(); i++) {
            if (isUpperCase(word.charAt(i)) != prev) {
                return false;
            }
        }
        return true;
    }

    public boolean isUpperCase(char c) {
        return c >= 'A' && c <= 'Z';
    }
```