## [232. Implement Queue using Stacks](https://leetcode-cn.com/problems/implement-queue-using-stacks/)

## 1. 题目描述\(简单\)

Implement the following operations of a queue using stacks.

* push\(x\) -- Push element x to the back of queue.
* pop\(\) -- Removes the element from in front of queue.
* peek\(\) -- Get the front element.
* empty\(\) -- Return whether the queue is empty.

Example:

```
MyQueue queue = new MyQueue();

queue.push(1);
queue.push(2);  
queue.peek();  // returns 1
queue.pop();   // returns 1
queue.empty(); // returns false
```

**Notes**:

* You must use only standard operations of a stack -- which means only push to top, peek/pop from top, size, and is empty operations are valid.
* Depending on your language, stack may not be supported natively. You may simulate a stack by using a list or deque \(double-ended queue\), as long as you use only standard operations of a stack.
* You may assume that all operations are valid \(for example, no pop or peek operations will be called on an empty queue\).

## 2. 思路

入队：栈1压栈push即可  
出队：利用辅助栈出队，实现先入先出  
队首元素：辅助栈的栈顶  
判空：两个栈均为空

## 3. 解决方法

### 3.1

```java
public class MyQueue {
    private Stack<Integer> stack1;
    private Stack<Integer> stack2;

    /** Initialize your data structure here. */
    public MyQueue() {
        stack1 = new Stack<>();
        stack2 = new Stack<>();
    }

    /** Push element x to the back of queue. */
    public void push(int x) {
        stack1.push(x);
    }

    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        if(stack2.isEmpty()) {
            while(!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
        }
        return stack2.pop();
    }

    /** Get the front element. */
    public int peek() {
        if(stack2.isEmpty()) {
            while(!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
        }
        return stack2.peek();
    }

    /** Returns whether the queue is empty. */
    public boolean empty() {
        return stack1.isEmpty()&&stack2.isEmpty();
    }
}
```

* 入队push

```java
    public void push(int x) {
            stack1.push(x);
    }
```

时间复杂度：$$O(1)$$
空间复杂度：$$O(1)$$

* 出队pop

```java
    public int pop() {
        if(stack2.isEmpty()) {
            while(!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
        }
        return stack2.pop();
    }
```

时间复杂度：$$O(n)$$
空间复杂度：$$O(1)$$

* 队首元素peek

```java
    public int peek() {
        if(stack2.isEmpty()) {
            while(!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
        }
        return stack2.peek();
    }
```
时间复杂度：$$O(n)$$  
空间复杂度：$$O(1)$$

* 判空empty

```java
    public boolean empty() {
        return stack1.isEmpty()&&stack2.isEmpty();
    }
```

时间复杂度：$$O(1)$$  
空间复杂度：$$O(1)$$



