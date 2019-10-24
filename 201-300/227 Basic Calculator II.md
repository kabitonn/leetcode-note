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
Example 2:
```
Input: " 3/2 "
Output: 1
```
Example 3:
```
Input: " 3+5 / 2 "
Output: 5
```
**Note**:

- You may assume that the given expression is always valid.
- Do not use the eval built-in library function.

## 思路


- 栈
- 中缀转后缀表达式
- 递归下降(词法分析)


## 解决方法

### 两个栈(数据+操作符)

1. 用stack nums记录操作数，用stack ops记录操作符
- 顺序遍历，按顺序读取每个char。
    1. 空格再见。
    2. 如果是数字，记录
    3. 运算符，开始计算
        1. 上次的操作符是*/，那么上个操作数和当前刚拿到的第二操作数都在，直接计算这一步的结果，然后把他们出栈。
        2. 判断栈和当前运算符
            1. 如果当前操作符是*/，那么不管上个操作数是+还是-，操作数和操作符都要入栈。因为优先级高。
            2. 当然如果你上个操作数是空，也要入栈。
            3. 剩下一种情况，就是上个操作符是+-了，计算结果入栈，新操作符也入栈
        3. 数值归零，以待重新获取
- 因为遇到操作符才会计算，为了在循环里一同处理，在字符串后面加个操作符。
- 遍历完毕的时候，nums顶为计算结果。

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

```java
    public int calculate(String s) {
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

数据栈保存表达式中的和因子，最后统一计算求和
只需关注上一个运算符
sign记录前一个运算符，若是 + - 将操作数入栈，若是 * / 取出上个操作数和当前计算并入栈

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

### 实时计算，乘除回退

默认表达式第一个操作数为0，第一个操作符为+，向后遍历获取第二个操作数，直接计算

+ - 计算可以直接计算，需要将右操作数记录为左操作数留待后续可能的回退操作
* / 计算需要先回退左操作数的运算，然后再将左操作数和右操作数计算加入结果中

```java
    public int calculate2(String s) {
        int num = 0;
        String str = s + '+';
        int res = 0;
        int[] left = {0};
        char sign = '+';
        for (char c : str.toCharArray()) {
            if (c == ' ') {
                continue;
            }
            if (c >= '0' && c <= '9') {
                num = num * 10 + c - '0';
                continue;
            }
            res = compute(left, num, sign, res);
            sign = c;
            num = 0;
        }
        return res;
    }

    public int compute(int[] left, int right, char operator, int res) {

        if (operator == '+') {
            res += right;
            left[0] = right;
        }
        if (operator == '-') {
            res -= right;
            left[0] = -right;
        }
        if (operator == '*') {
            res = res - left[0] + left[0] * right;
            left[0] = left[0] * right;
        }
        if (operator == '/') {
            res = res - left[0] + left[0] / right;
            left[0] = left[0] / right;
        }
        return res;
    }
```

### 转为后缀表达式(逆波兰式)求值


1. 任何中缀表达式都由运算数，运算符，括号（大，中，小），这三部分组成。

2. 从中缀表达式的左边开始扫描（脑中自己想像的），若遇到运算数时，则直接将其输出（不压入堆栈）。

3. 若遇到左括号，则将其压栈。

4. 若遇到右括号，表达括号内的中缀表达式已经扫描完毕。这时需将栈顶的运算符依次弹出并输出，直至遇到左括号[左括号弹出但不输出]。

5. 若遇到的是运算符：
    a. 如果该运算符的优先级大于栈顶运算符的优先级时，将其压栈
    b. 如果该运算符的优先级小于等于栈顶运算符的优先级时，将栈顶运算符弹出并输出，接着和新的栈顶运算符比较，若大于，则将其压栈，若小于等于，继续将栈顶运算符弹出并输出......(一直递归下去，直至运算符大于栈顶云算符为止)。

6. 最后一步，若扫描到中缀表达式的末尾[即扫描结束]，若堆栈中还有存留的运算符依次弹出并输出即可。


```java
    public int calculate3(String s) {
        List<String> reversePolishNotationToken = infixToSuffix(s);
        System.out.println(reversePolishNotationToken);
        return evalSuffix(reversePolishNotationToken);
    }

    public List<String> infixToSuffix(String s) {
        List<String> list = new ArrayList<>();
        char[] ops = new char[s.length() / 2];
        int top = 0;


        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (c == ' ') {
                continue;
            }
            if (c >= '0' && c <= '9') {
                String num = "";
                while (i < s.length() && s.charAt(i) >= '0' && s.charAt(i) <= '9') {
                    num += s.charAt(i++);
                }
                list.add(num);
                i--;
                continue;
            }
            if (c == '(') {
                ops[top++] = c;
            } else if (c == ')') {
                while (top > 0 && ops[top - 1] != '(') {
                    list.add("" + ops[--top]);
                }
                if (top > 0) {
                    --top;
                }
            } else {
                while (top > 0 && priority(c) <= priority(ops[top - 1])) {
                    list.add("" + ops[--top]);
                }
                ops[top++] = c;
            }
        }
        while (top > 0) {
            list.add("" + ops[--top]);
        }
        return list;
    }

    public int evalSuffix(List<String> strs) {
        int[] nums = new int[strs.size() / 2 + 1];
        int top = 0;
        int op2;
        for (String s : strs) {
            if ("+".equals(s)) {
                op2 = nums[--top];
                nums[top - 1] = (nums[top - 1] + op2);
            } else if ("-".equals(s)) {
                op2 = nums[--top];
                nums[top - 1] = (nums[top - 1] - op2);
            } else if ("*".equals(s)) {
                op2 = nums[--top];
                nums[top - 1] = (nums[top - 1] * op2);
            } else if ("/".equals(s)) {
                op2 = nums[--top];
                nums[top - 1] = (nums[top - 1] / op2);
            } else {
                nums[top++] = (Integer.valueOf(s));
            }
        }
        return nums[0];
    }


    private int priority(char c) {
        if (c == '+' || c == '-') {
            return 1;
        } else if (c == '*' || c == '/') {
            return 2;
        }
        return 0;
    }
```


### 递归下降

递归下降分析来求表达式的值。表达式的文法如下

```
expr = term {+、-} term
term = factor {*、/} factor
factor = (expr) or number
```

表达式中将空格去除

```java
    public int calculate4(String s) {
        s=s.replaceAll("\\s*","");
        return expression(s);
    }

    int curPos = 0;

    public int expression(String s) {
        int num1 = term(s);
        while (curPos < s.length() && (s.charAt(curPos) == '+' || s.charAt(curPos) == '-')) {
            char op = s.charAt(curPos++);
            int num2 = term(s);
            if (op == '+') {
                num1 += num2;
            } else {
                num1 -= num2;
            }
        }

        if (curPos < s.length() && s.charAt(curPos) == ')') {
            curPos++;
        }
        return num1;
    }

    public int term(String s) {
        int num1 = factor(s);
        while (curPos < s.length() && (s.charAt(curPos) == '*' || s.charAt(curPos) == '/')) {
            char op = s.charAt(curPos++);
            int num2 = factor(s);
            if (op == '*') {
                num1 *= num2;
            } else {
                num1 /= num2;
            }
        }
        return num1;
    }

    public int factor(String s) {
        if (s.charAt(curPos) == '(') {
            curPos++;
            return expression(s);
        }
        String str = "";
        while (curPos < s.length() && s.charAt(curPos) >= '0' && s.charAt(curPos) <= '9') {
            str += s.charAt(curPos++);
        }
        return Integer.valueOf(str);
    }
```

