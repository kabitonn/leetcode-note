
# 556. Next Greater Element III(M)
 
[556. 下一个更大元素 III](https://leetcode-cn.com/problems/next-greater-element-iii/)

## 题目描述(中等)

给定一个32位正整数 n，你需要找到最小的32位整数，其与 n 中存在的位数完全相同，并且其值大于n。如果不存在这样的32位整数，则返回-1。

示例 1:
```
输入: 12
输出: 21
```

示例 2:
```
输入: 21
输出: -1
```

## 思路

转化为数组，求下一个排列数的数组

## 解决方法

### 下一个排列数

[下一个排列](../001-100/031_Next_Permutation.md)

从后往前找到第一个不再递增的数，从该位置起向后找到比该数大的最小的值，交换两数位置，逆序第一个数后面的部

```java
    public int nextGreaterElement(int n) {
        String str = n + "";
        char[] chars = str.toCharArray();
        int len = chars.length;
        int i = len - 2;
        while (i >= 0 && chars[i] >= chars[i + 1]) {
            i--;
        }
        if (i < 0) {
            return -1;
        }
        int j = i + 1;
        while (j < len && chars[j] > chars[i]) {
            j++;
        }
        j--;
        swap(chars, i, j);
        reverse(chars, i + 1);
        return valueOf(chars);
    }

    public int valueOf(char[] chars) {
        int num = 0;
        for (int i = 0; i < chars.length; i++) {
            if (num >= Integer.MAX_VALUE / 10) {
                return -1;
            }
            num = num * 10 + chars[i] - '0';
        }
        return num;
    }

    public void swap(char[] chars, int i, int j) {
        char c = chars[i];
        chars[i] = chars[j];
        chars[j] = c;
    }

    public void reverse(char[] chars, int i) {
        int j = chars.length - 1;
        while (i < j) {
            swap(chars, i++, j--);
        }
    }
```