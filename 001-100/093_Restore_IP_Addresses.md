# 093. Restore IP Addresses(M)
[](https://leetcode-cn.com/problems/restore-ip-addresses/)

## 题目描述(中等)

Given a string containing only digits, restore it by returning all possible valid IP address combinations.

Example:
```
Input: "25525511135"
Output: ["255.255.11.135", "255.255.111.35"]
```

## 思路

## 解决方法

- 遍历
- 回溯 递归

### 暴力遍历

遍历所有可能的位置划分

直接用利用三个指针将字符串强行分为四部分，遍历所有的划分，然后选取合法的解

```java
    public List<String> restoreIpAddresses(String s) {
        List<String> list = new ArrayList<>();
        int len = s.length();
        for (int a = 1; a < 4 && a < len - 2; a++) {
            for (int b = a + 1; b < a + 4 && b < len - 1; b++) {
                for (int c = b + 1; c < b + 4 && c < len; c++) {
                    if (len - c > 3) {
                        continue;
                    }
                    String A = (s.substring(0, a));
                    String B = (s.substring(a, b));
                    String C = (s.substring(b, c));
                    String D = (s.substring(c, len));
                    if (isValid(A) && isValid(B) && isValid(C) && isValid(D)) {
                        String ip = A + "." + B + "." + C + "." + D;
                        list.add(ip);
                    }
                }
            }
        }
        return list;
    }

    private boolean isValid(String s) {
        if ((s.charAt(0) == '0' && s.length() > 1) || Integer.parseInt(s) > 255) {
            return false;
        }
        return true;
    }

```


```java
    public List<String> restoreIpAddresses1(String s) {
        int len = s.length();
        List<String> list = new ArrayList<>();
        for (int a = 1; a <= 3; a++) {
            for (int b = 1; b <= 3; b++) {
                for (int c = 1; c <= 3; c++) {
                    for (int d = 1; d <= 3; d++) {
                        if (a + b + c + d != len) {
                            continue;
                        }
                        String A = (s.substring(0, a));
                        String B = (s.substring(a, a + b));
                        String C = (s.substring(a + b, a + b + c));
                        String D = (s.substring(a + b + c, len));
                        if (isValid(A) && isValid(B) && isValid(C) && isValid(D)) {
                            String ip = A + "." + B + "." + C + "." + D;
                            list.add(ip);
                        }
                    }
                }
            }
        }
        return list;
    }
```

时间复杂度：O(n)。

空间复杂度：O(1)。

### 回溯 递归

```java
    public List<String> restoreIpAddresses2(String s) {
        List<String> list = new ArrayList<>();
        backtrack(list, s, "", 0, 0);
        return list;
    }

    public void backtrack(List<String> list, String s, String ip, int index, int group) {

        if (s.length() - index > 3 * (4 - group)) {
            return;
        }
        if (index == s.length() && group == 4) {
            list.add(ip.substring(0,ip.length()-1));
            return;
        }
        if (index >= s.length() || group == 4) {
            return;
        }
        ip = ip + s.charAt(index);
        backtrack(list, s, ip + ".", index + 1, group + 1);
        if (s.charAt(index) == '0') {
            return;
        }
        if (index + 1 < s.length()) {
            ip = ip + s.charAt(index + 1);
            backtrack(list, s, ip + ".", index + 2, group + 1);
        }
        if (index + 2 < s.length()) {
            int num = Integer.valueOf(s.substring(index, index + 3));
            if (num > 0 && num <= 255) {
                ip = ip + s.charAt(index + 2);
                backtrack(list, s, ip + ".", index + 3, group + 1);
            }
        }
    }
```