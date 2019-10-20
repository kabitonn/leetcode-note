# 013. Roman to Integer(E)
[013. Roman to Integer](https://leetcode-cn.com/problems/roman-to-integer/)

## 题目描述\(简单\)

Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

For example, two is written as II in Roman numeral, just two one's added together. Twelve is written as, XII, which is simply X + II. The number twenty seven is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

> * I can be placed before V \(5\) and X \(10\) to make 4 and 9. 
>   -X can be placed before L \(50\) and C \(100\) to make 40 and 90. 
> * C can be placed before D \(500\) and M \(1000\) to make 400 and 900.
>   Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999.

Example 1:

```
Input: "III"
Output: 3
```

Example 2:

```
Input: "IV"
Output: 4
```

Example 3:

```
Input: "IX"
Output: 9
```

Example 4:

```
Input: "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.
```

Example 5:

```
Input: "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

## 思路

1. 针对可能是两个字符组成的罗马数字判断当前字符和下一个字符，其余只判断当前字符
2. 每次获取两个字符的组合判断是否有效，若有效相加有效值，若无效，获取单个字符对应有效值

## 解决方法

### 1. 遍历字符串，依次转换

```java
    public int romanToInt(String s) {
        int num=0;
        int n=0;
        for(int i=0;i<s.length();i++) {
            switch (s.charAt(i)) {
            case 'M':
                n = 1000;
                break;
            case 'D':
                n=500;
                break;
            case 'C':
                if(i+1<s.length()) {
                    if(s.charAt(i+1)=='D') {
                        n=400;
                        i++;
                    }
                    else if(s.charAt(i+1)=='M') {
                        n=900;
                        i++;
                    }
                    else {
                        n=100;
                    }
                }
                else {
                    n=100;
                }

                break;
            case 'L':
                n=50;
                break;
            case 'X':
                if(i+1<s.length()) {
                    if(s.charAt(i+1)=='L') {
                        n=40;
                        i++;
                    }
                    else if(s.charAt(i+1)=='C') {
                        n=90;
                        i++;
                    }
                    else {
                        n=10;
                    }
                }
                else {
                    n=10;
                }

                break;
            case 'V':
                n=5;
                break;
            case 'I':
                if(i+1<s.length()) {
                    if(s.charAt(i+1)=='V') {
                        n=4;
                        i++;
                    }
                    else if(s.charAt(i+1)=='X') {
                        n=9;
                        i++;
                    }
                    else {
                        n=1;
                    }
                }
                else {
                    n=1;
                }
                break;
            default:
                break;
            }
            num +=n;
        }
        return num;
    }
```
时间复杂度：O(n)，n 是字符串的长度。

空间复杂度：O(1)。

### 
依次取出两位判断是否特殊，否则为一位字母对应值

```java
    public int romanToInt(String s) {
        int num = 0;
        int n=0;
        Map<String,Integer> map=new HashMap<>();
        map.put("I", 1);
        map.put("IV", 4);
        map.put("V", 5);
        map.put("IX", 9);
        map.put("X", 10);
        map.put("XL", 40);
        map.put("L", 50);
        map.put("XC", 90);
        map.put("C", 100);
        map.put("CD", 400);
        map.put("D", 500);
        map.put("CM", 900);
        map.put("M", 1000);
        for(int i=0;i<s.length();) {
            if(i+1<s.length()&&map.containsKey(s.substring(i, i+2))){
                n = map.get(s.substring(i,i+2));
                i+=2;
            }
            else {
                n = map.get(s.substring(i,i+1));
                i++;
            }
            num+=n;
        }
        return num;
    }
```

### 3. 提前处理特殊情况

```java
    public int romanToInt2(String s) {
        int sum = 0;
        if (s.contains("IV")) {
            sum -= 2;
        }
        if (s.contains("IX")) {
            sum -= 2;
        }
        if (s.contains("XL")) {
            sum -= 20;
        }
        if (s.contains("XC")) {
            sum -= 20;
        }
        if (s.contains("CD")) {
            sum -= 200;
        }
        if (s.contains("CM")) {
            sum -= 200;
        }
        char c[] = s.toCharArray();
        int count = 0;
        for (; count <= s.length() - 1; count++) {
            if (c[count] == 'M') {
                sum += 1000;
            }
            if (c[count] == 'D') {
                sum += 500;
            }
            if (c[count] == 'C') {
                sum += 100;
            }
            if (c[count] == 'L') {
                sum += 50;
            }
            if (c[count] == 'X') {
                sum += 10;
            }
            if (c[count] == 'V') {
                sum += 5;
            }
            if (c[count] == 'I') {
                sum += 1;
            }
        }
        return sum;
    }
```



