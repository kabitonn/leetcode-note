# 155. Min Stack(E)
[155. Min Stack](https://leetcode-cn.com/problems/min-stack/)

## 题目描述(简单)

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

> - push(x) -- Push element x onto stack.
> - pop() -- Removes the element on top of the stack.
> - top() -- Get the top element.
> - getMin() -- Retrieve the minimum element in the stack.
 

Example:
```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> Returns -3.
minStack.pop();
minStack.top();      --> Returns 0.
minStack.getMin();   --> Returns -2.
```

## 思路

- 辅助栈
- 单个栈栈顶存放最小值

## 解决方法

### 辅助栈

![](/assets/101-200/155-s-1-1.gif)

```java
class MinStack {
    private Stack<Integer> stack;
    private Stack<Integer> min_stack;
    public MinStack() {
        stack = new Stack<>();
        min_stack = new Stack<>();
    }
    public void push(int x) {
        stack.push(x);
        if(min_stack.isEmpty() || x <= min_stack.peek())
            min_stack.push(x);
    }
    public void pop() {
        if(stack.pop().equals(min_stack.peek()))
            min_stack.pop();
    }
    public int top() {
        return stack.peek();
    }
    public int getMin() {
        return min_stack.peek();
    }
}

```

### 单个栈

入栈出栈判断最小值是否改变并进行栈操作


```java
class MinStack {
	
	private Deque<Integer> stack = null;
	public MinStack() {
        stack = new LinkedList<>();
    }
    
    public void push(int x) {
    	if(stack.isEmpty()) {
    		stack.push(x);
    		stack.push(x);
    	}
    	else {
    		int min = stack.peek();
    		stack.push(x);
    		min = x<min?x:min;
    		stack.push(min);
		}
    }
    
    public void pop() {
        stack.pop();
        stack.pop();
    }
    
    public int top() {
    	int min = stack.pop();
    	int top = stack.peek();
    	stack.push(min);
        return top;
    }
    
    public int getMin() {
        return stack.peek();
    }

}
```




