# 020. Valid Parentheses
[020. Valid Parentheses](https://leetcode-cn.com/problems/valid-parentheses/)

## 题目描述(简单)

Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

> An input string is valid if:
> - Open brackets must be closed by the same type of brackets.
> - Open brackets must be closed in the correct order.
Note that an empty string is also considered valid.

Example 1:
```
Input: "()"
Output: true
```
Example 2:
```
Input: "()[]{}"
Output: true
```
Example 3:
```
Input: "(]"
Output: false
```
Example 4:
```
Input: "([)]"
Output: false
```
Example 5:
```
Input: "{[]}"
Output: true
```

## 思路

1. 栈
2. 数组模拟栈

## 解决方法

### 栈



```java
	public boolean isValid(String s) {
		Stack<Character> bracket = new Stack<>();
		for(char c:s.toCharArray()) {
			switch (c) {
			case '(':
			case '[':
			case '{':
				bracket.push(c);
				break;
			case ')':
				if(!bracket.empty()&&bracket.peek()=='(') {
					bracket.pop();
				}
				else {
					return false;
				}
				break;
			case ']':
				if(!bracket.empty()&&bracket.peek()=='[') {
					bracket.pop();
				}
				else {
					return false;
				}
				break;
			case '}':
				if(!bracket.empty()&&bracket.peek()=='{') {
					bracket.pop();
				}
				else {
					return false;
				}
				break;
			default:
				break;
			}
		}
		return bracket.empty();
	}
```


### 模拟栈



```java
	public boolean isValid(String s) {
        char[] stack = new char[s.length()];
        int p = 0;
        for(char c:s.toCharArray()) {
			switch (c) {
			case '(':
			case '[':
			case '{':
				stack[p++] = c;
				break;
			case ')':
				if(p!=0&&stack[p-1]=='(') {
					p--;
				}
				else {
					return false;
				}
				break;
			case ']':
				if(p!=0&&stack[p-1]=='[') {
					p--;
				}
				else {
					return false;
				}
				break;
			case '}':
				if(p!=0&&stack[p-1]=='{') {
					p--;
				}
				else {
					return false;
				}
				break;
			default:
				break;
			}
		}
        return p==0?true:false;
    }
```



```java
	public boolean isValid(String s) {
        char[] stack = new char[s.length()];
        int p = 0;
        for(int i=0;i<s.length();i++) {
        	if(p==0) {
        		stack[p++]=s.charAt(i);
        	}
        	else if(isPair(stack[p-1],s.charAt(i))) {
        		p--;
        	}
        	else if(!isPair(stack[p-1],s.charAt(i))) {
        		stack[p++]=s.charAt(i);
			}
        }
        return p==0?true:false;
    }
	public boolean isPair(char a,char b) {
		boolean isPair = false;
		switch (b) {
		case ')':
			if(a=='(') {
				isPair = true;
			}
			else {
				isPair = false;
			}
			break;
		case ']':
			if(a=='[') {
				isPair = true;
			}
			else {
				isPair = false;
			}
			break;
		case '}':
			if(a=='{') {
				isPair = true;
			}
			else {
				isPair = false;
			}
			break;
		default:
			break;
		}
		return isPair;
	}
```





