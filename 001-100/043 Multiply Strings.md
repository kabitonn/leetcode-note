# 043. Multiply Strings(M)
[043. Multiply Strings](https://leetcode-cn.com/problems/multiply-strings/)

## 题目描述(中等)

Given two non-negative integers num1 and num2 represented as strings, return the product of num1 and num2, also represented as a string.

Example 1:
```
Input: num1 = "2", num2 = "3"
Output: "6"
```
Example 2:
```
Input: num1 = "123", num2 = "456"
Output: "56088"
```
**Note**:
1. The length of both num1 and num2 is < 110.
2. Both num1 and num2 contain only digits 0-9.
3. Both num1 and num2 do not contain any leading zero, except the number 0 itself.
4. You must not use any built-in BigInteger library or convert the inputs to integer directly.


## 思路

## 解决方法

### 

```java
    public String multiply(String num1, String num2) {
        String multiply = "0";
        char[] n1 = num1.toCharArray();
        char[] n2 = num2.toCharArray();
        for (int i = n1.length - 1; i >= 0; i--) {
            int a = n1[i] - '0';
            int carry = 0;
            StringBuilder sb = new StringBuilder();
            for (int j = n2.length - 1; j >= 0; j--) {
                int b = n2[j] - '0';
                int tmp = a * b + carry;
                sb.append(tmp % 10);
                carry = tmp / 10;
            }
            if (carry != 0) {
                sb.append(carry);
            }
            sb.reverse();
            for (int k = 0; k < n1.length - 1 - i; k++) {
                sb.append('0');
            }
            while(true){
                if(sb.charAt(0)=='0'&&sb.length()>1){
                    sb.deleteCharAt(0);
                }
                else {
                    break;
                }
            }
            String tmpMultiply = sb.toString();
            multiply = addString(tmpMultiply, multiply);
        }

        return multiply;

    }

    public String addString(String num1, String num2) {
        StringBuilder res = new StringBuilder("");
        int i = num1.length() - 1, j = num2.length() - 1, carry = 0;
        while (i >= 0 || j >= 0 || carry != 0) {
            int n1 = i >= 0 ? num1.charAt(i) - '0' : 0;
            int n2 = j >= 0 ? num2.charAt(j) - '0' : 0;
            int tmp = n1 + n2 + carry;
            carry = tmp / 10;
            res.append(tmp % 10);
            i--;
            j--;
        }

        return res.reverse().toString();
    }
```

时间复杂度：O(m * n)。m，n 是两个字符串的长度。

空间复杂度：O(m + n)。m，n 是两个字符串的长度。