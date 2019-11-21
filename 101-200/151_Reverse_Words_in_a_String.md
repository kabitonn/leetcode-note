# 151. Reverse Words in a String(M)


[151. 翻转字符串里的单词](https://leetcode-cn.com/problems/reverse-words-in-a-string/)


## 题目描述(中等)

Given an input string, reverse the string word by word.

Example 1:
```
Input: "the sky is blue"
Output: "blue is sky the"
```
Example 2:
```
Input: "  hello world!  "
Output: "world! hello"
Explanation: Your reversed string should not contain leading or trailing spaces.
```
Example 3:
```
Input: "a good   example"
Output: "example good a"
Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.
```

**Note**:

- A word is defined as a sequence of non-space characters.
- Input string may contain leading or trailing spaces. However, your reversed string should not contain leading or trailing spaces.
- You need to reduce multiple spaces between two words to a single space in the reversed string.
 

**Follow up**:

For C programmers, try to solve it in-place in O(1) extra space.


## 思路

- 分隔倒序


## 解决方法



### 分隔倒序



```java
    public String reverseWords(String s) {
        String[] strs = s.trim().split("\\s+");
        int i = 0, j = strs.length - 1;
        for (; i < j; i++, j--) {
            String tmp = strs[i];
            strs[i] = strs[j];
            strs[j] = tmp;
        }
        StringBuilder sb = new StringBuilder();
        for (int k = 0; k < strs.length; k++) {
            sb.append(strs[k]);
            if (k != strs.length - 1) {
                sb.append(" ");
            }
        }
        return sb.toString();
    }
```


```java
    public String reverseWords1(String s) {
        String[] strs = s.trim().split("\\s+");
        StringBuilder sb = new StringBuilder();
        for (int k = strs.length - 1; k >= 0; k--) {
            if (k == strs.length - 1) {
                sb.append(strs[k]);
            } else {
                sb.append(" " + strs[k]);
            }
        }
        return sb.toString();
    }
```


## Java中反斜线"\"

在java字符串和正则表达式中，“\”都具有特殊的含义

**一、在Java的字符串中"\"有两个功能**

- （一）代表特殊字符：\t代表制表符，\n代表换行....等。
- （二）代表转义，在字符串中，如果出现" ' \，会造成代码歧义，如：
        ```
        String s = "小明说："学习快乐"";//原来的一句话被分成两份
        Invalid escape sequence (valid ones are  \b  \t  \n  \f  \r  \"  \'  \\ )
        这时，就需要在造成歧义的字符前加\，来告诉编译器：这个字符只是一个普通字符。
        ```
        
    会造成歧义的有 \    '    "当我们想让他们代表普通字符的时候就需要变成\\    \'   \"
    
**二、在正则中”\”同样被赋予了两个功能**
　　
- （一）代表特殊功能的字符：如\d代表数组
- （二）代表转义，和上面一样，当出现字符歧义时，加上\表示普通字符。

**三、总结**
　　

　　因为" \ "号的在正则中被赋予了特殊含义，所以当我们想在正则中匹配"\"时，需要加上转义变成了"\\"。

　　在java字符串中，如果想表示两个普通字符"\\"，同样需要为”\”加上转义字符，变成了"\\\\"。

　　所以当我们想在java中使用正则表达式匹配"\"时，就需要写成"\\\\"





