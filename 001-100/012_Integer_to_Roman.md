# 012. Integer to Roman(M)
[012. Integer to Roman](https://leetcode-cn.com/problems/integer-to-roman/)

## 题目描述(中等)

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
> * X can be placed before L \(50\) and C \(100\) to make 40 and 90. 
> * C can be placed before D \(500\) and M \(1000\) to make 400 and 900.
>   Given an integer, convert it to a roman numeral. Input is guaranteed to be within the range from 1 to 3999.

Example 1:

```
Input: 3
Output: "III"
```

Example 2:

```
Input: 4
Output: "IV"
```

Example 3:

```
Input: 9
Output: "IX"
```

Example 4:

```
Input: 58
Output: "LVIII"
```

> Explanation: L = 50, V = 5, III = 3.

Example 5:

```
Input: 1994
Output: "MCMXCIV"
```

> Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.

## 思路

1. 最大的依此减去当前包含最大的值对应的字符
2. 直接对每一位数字转换为对应的字符

## 解决方法

### 依次取最大数值

```java
public String intToRoman(int num) {
    	StringBuilder sb = new StringBuilder();
    	while(num>=1000) {
    		sb.append("M");
    		num-=1000;
    	}
    	while(num>=900) {
    		sb.append("CM");
    		num-=900;
    	}
    	while(num>=500) {
    		sb.append("D");
    		num-=500;
    	}
    	while(num>=400) {
    		sb.append("CD");
    		num-=400;
    	}
    	while(num>=100) {
    		sb.append("C");
    		num-=100;
    	}
    	while(num>=90) {
    		sb.append("XC");
    		num-=90;
    	}
    	while(num>=50) {
    		sb.append("L");
    		num-=50;
    	}
    	while(num>=40) {
    		sb.append("XL");
    		num-=40;
    	}
    	while(num>=10) {
    		sb.append("X");
    		num-=10;
    	}
    	while(num>=9) {
    		sb.append("IX");
    		num-=9;
    	}
    	while(num>=5) {
    		sb.append("V");
    		num-=5;
    	}
    	while(num>=4) {
    		sb.append("IV");
    		num-=4;
    	}
    	while(num>=1) {
    		sb.append("I");
    		num-=1;
    	}
        return sb.toString();
    }
```

```java
    public String intToRoman(int num) {
        StringBuilder sb = new StringBuilder();
        int[] values = {1000,900,500,400,100,90,50,40,10,9,5,4,1};
        String[] strings = {"M","CM","D","CD","C","XC","L","XL","X","IX","V","IV","I"};
        for(int i=0;i<values.length;i++) {
            while(num>=values[i]) {
                sb.append(strings[i]);
                num-=values[i];
            }

        }
        return sb.toString();
    }
```

时间复杂度：不是很清楚，也许是 O(1)？因为似乎和问题规模没什么关系了。

空间复杂度：O(1).

### 依次取出每位值

```java
   public String intToRoman(int num) {
        String M[] = {"", "M", "MM", "MMM"};//0,1000,2000,3000
        String C[] = {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"};//0,100,200,300...
        String X[] = {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"};//0,10,20,30...
        String I[] = {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"};//0,1,2,3...
        return M[num/1000] + C[(num%1000)/100]+ X[(num%100)/10] + I[num%10];
    }
```

时间复杂度：O(1)或者说是 num 的位数，不是很确定。

空间复杂度：O(1)。


