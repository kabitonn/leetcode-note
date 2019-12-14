
# 372. Super Pow(M)


[372. 超级次方](https://leetcode-cn.com/problems/super-pow/)

## 题目描述(中等)

你的任务是计算 a^b 对 1337 取模，a 是一个正整数，b 是一个非常大的正整数且会以数组形式给出。

示例 1:
```
输入: a = 2, b = [3]
输出: 8
```
示例 2:
```
输入: a = 2, b = [1,0]
输出: 1024
```

## 思路

- 循环
- 快速幂
- 降幂

## 解决方法

### 循环求解

$(a*b) \% c = (a \% c)(b \% c) \% c$

a 是每轮循环的底数即 $a = a^{(i-1)*10}$

```java
    public int superPow1(int a, int[] b) {
        int n = b.length;
        int[] r = new int[n];
        int mod = 1337;
        a %= mod;
        for (int i = 0; i < n; i++) {
            r[i] = 1;
            for (int j = 0; j < b[n - 1 - i]; j++) {
                r[i] = r[i] * a % mod;
            }
            int tmp = a;
            for (int j = 1; j < 10; j++) {
                a = a * tmp % mod;
            }
        }
        int pow = 1;
        for (int i = 0; i < n; i++) {
            pow *= r[i];
            pow %= mod;
        }
        return pow;
    }
```

### 快速幂

```java
    public int superPow2(int a, int[] b) {
        int n = b.length;
        int r = 1;
        int mod = 1337;
        a %= mod;
        for (int i = 0; i < n; i++) {
            r = r * fastPow(a, b[n - 1 - i], mod) % mod;
            a = fastPow(a, 10, mod);
        }
        return r;
    }

    public int fastPow(int x, int y, int m) {
        int r = 1;
        while (y != 0) {
            if ((y & 1) == 1) {
                r = r * x % m;
            }
            x = x * x % m;
            y >>= 1;
        }
        return r;
    }
```

### 欧拉函数+降幂函数+快速幂

降幂公式：
$A^BmodC=A^{Bmod(phi(C))+phi(C)}modC$


```java
    public int superPow3(int a, int[] b) {
        int mod = 1337;
        int c = phi(mod);
        int sum = 0;
        for (int i = 0; i < b.length; i++) {
            sum = (sum * 10 + b[i]) % c;
        }
        sum += c;
        a = a % mod;
        return fastPow(a, sum, mod);
    }


    private int phi(int x) {
        int phi = x;
        for (int i = 2; i * i <= x; i++) {
            if (x % i == 0) {
                phi = phi - phi / i;
                while (x % i == 0) {
                    x /= i;
                }
            }
        }
        if (x > 1) {
            phi = phi - phi / x;
        }
        return phi;
    }
    public int fastPow(int x, int y, int m) {
        int r = 1;
        while (y != 0) {
            if ((y & 1) == 1) {
                r = r * x % m;
            }
            x = x * x % m;
            y >>= 1;
        }
        return r;
    }
```