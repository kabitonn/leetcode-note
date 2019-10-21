# 071. Simplify Path(M)
[071. 简化路径](https://leetcode-cn.com/problems/simplify-path/)


## 题目描述(中等)

Given an absolute path for a file (Unix-style), simplify it. Or in other words, convert it to the canonical path.

In a UNIX-style file system, a period . refers to the current directory. Furthermore, a double period .. moves the directory up a level. For more information, see: Absolute path vs relative path in Linux/Unix

Note that the returned canonical path must always begin with a slash /, and there must be only a single slash / between two directory names. The last directory name (if it exists) must not end with a trailing /. Also, the canonical path must be the shortest string representing the absolute path.

 

Example 1:
```
Input: "/home/"
Output: "/home"
Explanation: Note that there is no trailing slash after the last directory name.
```
Example 2:
```
Input: "/../"
Output: "/"
Explanation: Going one level up from the root directory is a no-op, as the root level is the highest level you can go.
```
Example 3:
```
Input: "/home//foo/"
Output: "/home/foo"
Explanation: In the canonical path, multiple consecutive slashes are replaced by a single one.
```
Example 4:
```
Input: "/a/./b/../../c/"
Output: "/c"
```
Example 5:
```
Input: "/a/../../b/../c//.//"
Output: "/c"
```
Example 6:
```
Input: "/a//b////c/d//././/.."
Output: "/a/b/c"
```



## 思路

生成一个绝对路径，把相对路径中 ".." 和 "." 都转换为实际的路径，此外，"///" 多余的 "/" 要去掉，开头要有一个 "/"，末尾不要 "/"。

## 解决方法

### 以分隔符'/'拆分成数组判断
```java
    public String simplifyPath(String path) {
        if (path == null || path.length() == 0) {
            return "/";
        }
        String[] strs = path.split("/");
        List<String> wordList = new ArrayList<>();
        for (int i = 0; i < strs.length; i++) {
            if (strs[i].equals("..")) {
                if (!wordList.isEmpty()) {
                    wordList.remove(wordList.size()-1);
                }
            } else if (!strs[i].equals("") && !strs[i].equals(".")) {
                wordList.add(strs[i]);
            }
        }
        if (wordList.isEmpty()) {
            return "/";
        }
        StringBuilder sb = new StringBuilder();
        for (String s : wordList) {
            sb.append("/" + s);
        }
        return sb.toString();
    }
```
