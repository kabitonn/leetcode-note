
# 468. Validate IP Address(M)

[]()

## 题目描述(中等)

## 思路

- 分类判断

## 解决方法

### 分类判断

`split(regex, limit)` 用法
- regex:正则表达式或字符串
- 控制模式应用的次数 
  - limit =0 表示模式应用尽可能多的次数，数组可以是任意长度，并且结尾空字符串将被丢弃。
  - limit>0时 那么模式将会应用limit-1次 数组长度不会超过limit
  -  limit为非正整数表示 模式被应用尽可能多的次数 比如-1

```java
    public String validIPAddress(String IP) {
        if (IP.contains(".")) {
            String[] strs = IP.split("\\.", -1);
            if (strs.length != 4) {
                return result(0);
            }
            for (int i = 0; i < strs.length; i++) {
                if (strs[i].length() == 0) {
                    return result(0);
                }
                int num = 0;
                for (int j = 0; j < strs[i].length(); j++) {
                    char c = strs[i].charAt(j);
                    if (j == 0 && c == '0' && j < strs[i].length() - 1) {
                        return result(0);
                    }
                    if (c < '0' || c > '9') {
                        return result(0);
                    }
                    num = num * 10 + (c - '0');
                    if (num > 255) {
                        return result(0);
                    }
                }
            }
            return result(1);
        } else if (IP.contains(":")) {
            String[] strs = IP.toUpperCase().split(":", -1);
            if (strs.length != 8) {
                return result(0);
            }
            for (int i = 0; i < strs.length; i++) {
                if (strs[i].length() == 0 || strs[i].length() > 4) {
                    return result(0);
                }
                for (int j = 0; j < strs[i].length(); j++) {
                    char c = strs[i].charAt(j);
                    if ((c < '0' || c > '9') && (c < 'A' || c > 'F')) {
                        return result(0);
                    }
                }
            }
            return result(2);
        } else {
            return result(0);
        }
    }

    public String result(int r) {
        if (r == 1) {
            return "IPv4";
        } else if (r == 2) {
            return "IPv6";
        } else {
            return "Neither";
        }
    }
```