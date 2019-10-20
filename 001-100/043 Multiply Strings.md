# 043. Multiply Strings\(M\)

[043. Multiply Strings](https://leetcode-cn.com/problems/multiply-strings/)

## 题目描述\(中等\)

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
1. The length of both num1 and num2 is &lt; 110.  
2. Both num1 and num2 contain only digits 0-9.  
3. Both num1 and num2 do not contain any leading zero, except the number 0 itself.  
4. You must not use any built-in BigInteger library or convert the inputs to integer directly.

## 思路

## 解决方法

### 模仿乘法计算

![](/assets/001-100/043-s-1-1.png)  
个位乘个位，得出一个数，然后个位乘十位，全部乘完以后，就再用十位乘以各个位。然后百位乘以各个位，最后将每次得出的数相加。十位的结果要补 1 个 0 ，百位的结果要补两个 0

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

时间复杂度：O\(m \* n\)。m，n 是两个字符串的长度。

空间复杂度：O\(1\)。

### 统一更新进位


![](/assets/001-100/043-s-2-1.png)

把进位先不算，写到对应的位置。最后再统一更新 pos 中的每一位。而对于运算中的每个结果，可以观察出一个结论。
num1 的第 i 位乘上 num2 的第 j 位，结果会分别对应 pos 的第 i + j 位和第 i + j + 1 位。

例如图中的红色部分，num1 的第 1 位乘上 num2 的第 0 位，结果就对应 pos 的第 1 + 0 = 1 和 1 + 0 + 1 = 2 位。
有了这一点，我们就可以遍历求出每一个结果，然后更新 pos 上的值就够了。

```java
    public String multiply1(String num1, String num2) {
        if (num1.equals("0") || num2.equals("0")) {
            return "0";
        }
        int n1 = num1.length();
        int n2 = num2.length();
        int[] pos = new int[n1 + n2];
        for (int i = n1 - 1; i >= 0; i--) {
            for (int j = n2 - 1; j >= 0; j--) {
                //相乘的结果
                int mul = (num1.charAt(i) - '0') * (num2.charAt(j) - '0');
                //加上 pos[i+j+1] 之前已经累加的结果
                int sum = mul + pos[i + j + 1];
                //更新 pos[i + j]
                pos[i + j] += sum / 10;
                //更新 pos[i + j + 1]
                pos[i + j + 1] = sum % 10;
            }
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < pos.length; i++) {
            //判断最高位是不是 0
            if (i == 0 && pos[i] == 0) {
                continue;
            }
            sb.append(pos[i]);
        }
        return sb.toString();
    }
```

