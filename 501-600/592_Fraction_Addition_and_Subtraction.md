
# 592. Fraction Addition and Subtraction(M)
 
[592. 分数加减运算](https://leetcode-cn.com/problems/fraction-addition-and-subtraction/)

## 题目描述(中等)

给定一个表示分数加减运算表达式的字符串，你需要返回一个字符串形式的计算结果。 这个结果应该是不可约分的分数，即最简分数。 如果最终结果是一个整数，例如 2，你需要将它转换成分数形式，其分母为 1。所以在上述例子中, 2 应该被转换为 2/1。

示例 1:
```
输入:"-1/2+1/2"
输出: "0/1"
```
示例 2:
```
输入:"-1/2+1/2+1/3"
输出: "1/3"
```
示例 3:
```
输入:"1/3-1/2"
输出: "-1/6"
```
示例 4:
```
输入:"5/3+1/3"
输出: "2/1"
```
**说明**:
- 输入和输出字符串只包含 '0' 到 '9' 的数字，以及 '/', '+' 和 '-'。 
- 输入和输出分数格式均为 ±分子/分母。如果输入的第一个分数或者输出的分数是正数，则 '+' 会被省略掉。
- 输入只包含合法的最简分数，每个分数的分子与分母的范围是  [1,10]。 如果分母是1
- 意味着这个分数实际上是一个整数。
- 输入的分数个数范围是 [1,10]。
- 最终结果的分子与分母保证是 32 位整数范围内的有效整数。


## 思路

## 解决方法

### 逐个相加

- 首先每个判断分数的正负，确定分子的正负

```java
    public String fractionAddition(String expression) {
        int[] frac1 = {0, 1};
        char[] s = (expression).toCharArray();
        int n = s.length;
        int j = 0;
        while (j < n) {
            boolean negative = false;
            if (s[j] == '-') {
                negative = true;
                j++;
            } else if (s[j] == '+') {
                j++;
            }
            int num1 = 0;
            while (s[j] != '/') {
                num1 = num1 * 10 + s[j++] - '0';
            }
            if (negative) {
                num1 = -num1;
            }
            j++;
            int num2 = 0;
            while (j < n && (s[j] != '+' && s[j] != '-')) {
                num2 = num2 * 10 + s[j++] - '0';
            }
            int[] frac2 = {num1, num2};
            frac1 = addition(frac1, frac2);
        }
        return frac1[0] + "/" + frac1[1];
    }

    public int[] addition(int[] frac1, int[] frac2) {
        int n = frac1[1] * frac2[1];
        int m = frac1[0] * frac2[1] + frac2[0] * frac1[1];
        if (m == 0) {
            return new int[]{0, 1};
        }
        int gcd = greatestCommonDivisor(Math.abs(m), n);
        return new int[]{m / gcd, n / gcd};
    }

    public int greatestCommonDivisor(int a, int b) {
        int r;
        while (b != 0) {
            r = a % b;
            a = b;
            b = r;
        }
        return a;
    }
```

### 全部通分相加

记录每个分数的正负

```java
    public int lowestCommonMulti(int a, int b) {
        return a * b / greatestCommonDivisor(a, b);
    }

    public String fractionAddition1(String expression) {
        String[] substrs = expression.split("\\+|-");
        if (substrs[0].length() == 0) {
            substrs = Arrays.copyOfRange(substrs, 1, substrs.length);
        }
        boolean[] sign = new boolean[substrs.length];
        char[] s = expression.toCharArray();
        if (s[0] != '-') {
            sign[0] = true;
        }
        int count = 1;
        for (int i = 1; i < s.length; i++) {
            if (s[i] == '+') {
                sign[count++] = true;
            } else if (s[i] == '-') {
                sign[count++] = false;
            }
        }
        count = 0;
        int[] num = new int[sign.length];
        int[] den = new int[sign.length];
        for (String str : substrs) {
            String[] frac = str.split("/");
            num[count] = Integer.valueOf(frac[0]);
            den[count] = Integer.valueOf(frac[1]);
            count++;
        }
        int n = 1;
        for (int d : den) {
            n = lowestCommonMulti(n, d);
        }
        int m = 0;
        for (int i = 0; i < sign.length; i++) {
            if (sign[i]) {
                m += num[i] * n / den[i];
            } else {
                m -= num[i] * n / den[i];
            }
        }
        int gcd = greatestCommonDivisor(Math.abs(m), n);
        m /= gcd;
        n /= gcd;
        return m + "/" + n;
    }
```