# 227. Basic Calculator II(M)

[227. 基本计算器 II](https://leetcode-cn.com/problems/basic-calculator-ii/)

## 题目描述(中等)

Implement a basic calculator to evaluate a simple expression string.

The expression string contains only **non-negative** integers, **+**, **-**, *, / operators and empty spaces . The integer division should truncate toward zero.

Example 1:
```
Input: "3+2*2"
Output: 7
Example 2:
```

## 思路


- 栈

## 解决方法

### 两个栈(数据+操作符)


```java
    public int calculate0(String s) {
        Stack<Integer> nums = new Stack<>();
        Stack<Character> ops = new Stack<>();
        int num = 0;
        String str = s + "+";
        for (char c : str.toCharArray()) {
            if (c == ' ') {
                continue;
            }
            if (c >= '0' && c <= '9') {
                num = num * 10 + c - '0';
                continue;
            }
            if (!nums.isEmpty() && !ops.isEmpty()) {
                char op = ops.peek();
                if (op == '*') {
                    num = nums.pop() * num;
                    ops.pop();
                } else if (op == '/') {
                    num = nums.pop() / num;
                    ops.pop();
                }
            }
            if (nums.isEmpty() || ops.isEmpty() || c == '*' || c == '/') {

            } else {
                char op = ops.pop();
                if (op == '+') {
                    num = nums.pop() + num;
                } else if (op == '-') {
                    num = nums.pop() - num;
                }
            }
            nums.push(num);
            ops.push(c);
            num = 0;
        }
        return nums.pop();
    }

```


数组替代栈

```javapublic int calculate(String s) {
        int[] nums = new int[s.length()];
        char[] ops = new char[s.length()];
        int topNum = 0, topOp = 0;
        int num = 0;
        String str = s + "+";
        for (char c : str.toCharArray()) {
            if (c == ' ') {
                continue;
            }
            if (c >= '0' && c <= '9') {
                num = num * 10 + c - '0';
                continue;
            }
            char op = c;
            if (topOp != 0) {
                char preOp = ops[topOp - 1];
                if (preOp == '*') {
                    num = nums[--topNum] * num;
                    topOp--;
                } else if (preOp == '/') {
                    num = nums[--topNum] / num;
                    topOp--;
                }
            }
            if (topOp == 0 || op == '*' || op == '/') {

            } else {
                char preOp = ops[--topOp];
                if (preOp == '+') {
                    num = nums[--topNum] + num;
                } else if (preOp == '-') {
                    num = nums[--topNum] - num;
                }
            }
            nums[topNum++] = num;
            ops[topOp++] = op;
            num = 0;
        }
        return nums[--topNum];
    }
```

### 一个数据栈

```java
    public int calculate1(String s) {
        int[] nums = new int[s.length()];
        int top = 0;
        int num = 0;
        char sign = '+';
        String str = s + '+';
        for (char c : str.toCharArray()) {
            if (c == ' ') {
                continue;
            }
            if (c >= '0' && c <= '9') {
                num = num * 10 + c - '0';
                continue;
            }
            if (sign == '+') {
                nums[top++] = num;
            } else if (sign == '-') {
                nums[top++] = -num;
            } else if (sign == '*') {
                nums[top - 1] = nums[top - 1] * num;
            } else if (sign == '/') {
                nums[top - 1] = nums[top - 1] / num;
            }
            sign = c;
            num = 0;
        }
        int result = 0;
        for (int i = 0; i < top; i++) {
            result += nums[i];
        }
        return result;
    }
```