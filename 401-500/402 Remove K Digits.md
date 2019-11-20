
# 402. Remove K Digits(M)

[402. 移掉K位数字](https://leetcode-cn.com/problems/remove-k-digits/)

## 题目描述(中等)

给定一个以字符串表示的**非负整数** num，移除这个数中的 k 位数字，使得剩下的数字最小。

**注意**:
- num 的长度小于 10002 且 ≥ k。
- num 不会包含任何前导零。

示例 1 :
```
输入: num = "1432219", k = 3
输出: "1219"
解释: 移除掉三个数字 4, 3, 和 2 形成一个新的最小的数字 1219。
```
示例 2 :
```
输入: num = "10200", k = 1
输出: "200"
解释: 移掉首位的 1 剩下的数字为 200. 注意输出不能有任何前导零。
```
示例 3 :
```
输入: num = "10", k = 2
输出: "0"
解释: 从原数字移除所有的数字，剩余为空就是0。
```

## 思路

- 循环遍历移除
- 单调栈

## 解决方法

### 循环遍历移除

- 自前向后寻找第一个大于后者的数字，作为首要移除的数字
  - 若找不到，则删除最后一个数字
  - 若删除后首位为 0 ，去除首部的0


```java
    public String removeKdigits(String num, int k) {
        if (k > num.length()) {
            return "0";
        }
        for (int i = 0; i < k && num.length() > 0; i++) {
            boolean remove = false;
            for (int j = 0; j < num.length() - 1; j++) {
                if (num.charAt(j) > num.charAt(j + 1)) {
                    num = num.substring(0, j) + num.substring(j + 1);
                    remove = true;
                    break;
                }
            }
            if (!remove) {
                num = num.substring(0, num.length() - (k - i));
                break;
            }
            int j = 0;
            while (j < num.length() && num.charAt(j) == '0') {
                j++;
            }
            num = num.substring(j);
        }
        return num.length() == 0 ? "0" : num;
    }
```

### 单调栈

保持栈中元素处于递增状态
- 当前数字若大于栈顶数字，入栈
- 当前数字小于栈顶数字，栈顶数字出栈，即代表移除

所有元素入栈后
1. 移除元素剩余，继续出栈移除
2. 去除首部的0
3. 构造字符串

```java
    public String removeKdigits1(String num, int k) {
        char[] stack = new char[num.length()];
        int top = 0;
        for (char c : num.toCharArray()) {
            while (top > 0 && k > 0 && stack[top - 1] > c) {
                top--;
                k--;
            }
            stack[top++] = c;
        }
        while (top > 0 && k > 0) {
            top--;
            k--;
        }
        StringBuilder sb = new StringBuilder();
        int i = 0;
        while (i < top && stack[i] == '0') {
            i++;
        }
        while (i < top) {
            sb.append(stack[i++]);
        }
        return sb.length() == 0 ? "0" : sb.toString();
    }

```