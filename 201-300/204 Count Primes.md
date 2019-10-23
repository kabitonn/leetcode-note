# 204. Count Primes(E)
[204. Count Primes](https://leetcode-cn.com/problems/count-primes/)

## 题目描述(简单)

Count the number of prime numbers less than a non-negative number, n.

Example:
```
Input: 10
Output: 4
Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.
```


## 思路

## 解决方法

### 遍历


```java
    public int countPrimes(int n) {
        int count = 0;
        if (n > 2) {
            count++;
        }
        for (int i = 3; i < n; i += 2) {
            if (isPrime(i)) {
                count++;
            }
        }
        return count;
    }

    public boolean isPrime(int n) {
        for (int i = 3; i <= Math.sqrt(n)+1; i+=2) {
            if (n % i == 0) {
                return false;
            }
        }
        return true;
    }
```

### 厄拉多塞筛法

先将 2~n 的各个数放入表中，然后在2的上面画一个圆圈，然后划去2的其他倍数；第一个既未画圈又没有被划去的数是3，将它画圈，再划去3的其他倍数；现在既未画圈又没有被划去的第一个数 是5，将它画圈，并划去5的其他倍数……依次类推，一直到所有小于或等于 n 的各数都画了圈或划去为止。这时，表中画了圈的以及未划去的那些数正好就是小于n的素数。


```java
    public int countPrimes(int n) {
        int count = 0;
        boolean[] notPrime = new boolean[n];
        for (int i = 2; i < n; i++) {
            if (!notPrime[i]) {
                count++;
                for (int j = i * 2; j < n; j += i) {
                    notPrime[j] = true;
                }
            }
        }
        return count;
    }
```


