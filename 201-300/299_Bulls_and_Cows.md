# 299. Bulls and Cows(M)

[299. 猜数字游戏](https://leetcode-cn.com/problems/bulls-and-cows/)

## 题目描述(中等)

你正在和你的朋友玩 猜数字（Bulls and Cows）游戏：你写下一个数字让你的朋友猜。每次他猜测后，你给他一个提示，告诉他有多少位数字和确切位置都猜对了（称为“Bulls”, 公牛），有多少位数字猜对了但是位置不对（称为“Cows”, 奶牛）。你的朋友将会根据提示继续猜，直到猜出秘密数字。

请写出一个根据秘密数字和朋友的猜测数返回提示的函数，用 A 表示公牛，用 B 表示奶牛。

请注意秘密数字和朋友的猜测数都可能含有重复数字。

示例 1:
```
输入: secret = "1807", guess = "7810"
输出: "1A3B"
解释: 1 公牛和 3 奶牛。公牛是 8，奶牛是 0, 1 和 7。
```

示例 2:
```
输入: secret = "1123", guess = "0111"
输出: "1A1B"
解释: 朋友猜测数中的第一个 1 是公牛，第二个或第三个 1 可被视为奶牛。
```
说明: 你可以假设秘密数字和朋友的猜测数都只包含数字，并且它们的长度永远相等。

## 思路

- 遍历统计

## 解决方法

### 两次遍历字符串

第一次对所猜字符串遍历统计位置和数字都对的个数A，以及统计其余数字的个数  
第二次对秘密字符串遍历，统计位置不对，但有多余的相同数字的个数B

```java
    public String getHint(String secret, String guess) {
        int[] nums = new int[10];
        int A = 0, B = 0;
        for (int i = 0; i < guess.length(); i++) {
            char c = guess.charAt(i);
            if (i < secret.length() && c == secret.charAt(i)) {
                A++;
            } else {
                nums[c - '0']++;
            }
        }
        for (int i = 0; i < secret.length(); i++) {
            char c = secret.charAt(i);
            if (i >= guess.length() || c != guess.charAt(i)) {
                if (nums[c - '0'] > 0) {
                    B++;
                    nums[c - '0']--;
                }
            }
        }
        return A + "A" + B + "B";
    }
```

### 遍历字符串统计+遍历数字集合

第一轮遍历统计位置和数字相同的个数A，以及统计所猜字符串中剩余每种数字的个数，  
第二轮遍历每种数字剩余值是否大于0，大于0的个数为位置不同且数字不对的个数

```java
    public String getHint2(String secret, String guess) {
        int[] nums = new int[10];
        int A = 0, B = 0;
        for (int i = 0; i < secret.length(); i++) {
            char c1 = secret.charAt(i);
            char c2 = guess.charAt(i);
            if (c1 == c2) {
                A++;
            } else {
                nums[c1 - '0']--;
                nums[c2 - '0']++;
            }
        }
        for (int i = 0; i < 10; i++) {
            if (nums[i] > 0) {
                B += nums[i];
            }
        }
        B = secret.length() - A - B;
        return A + "A" + B + "B";
    }

```

第一轮遍历统计位置和数字相同的个数A，以及统计所猜字符串和秘密字符串中剩余每种数字的个数， 
第二轮遍历数字集合中位置不同，但存在相同数字的个数B

```java
    public String getHint1(String secret, String guess) {
        int[] buck1 = new int[10];
        int[] buck2 = new int[10];
        int A = 0, B = 0;
        for (int i = 0; i < secret.length(); i++) {
            char c1 = secret.charAt(i);
            char c2 = guess.charAt(i);
            if (c1 == c2) {
                A++;
            } else {
                buck1[c1 - '0']++;
                buck2[c2 - '0']++;
            }
        }
        for (int i = 0; i < 10; i++) {
            B += Math.min(buck1[i], buck2[i]);
        }
        return A + "A" + B + "B";
    }
```

