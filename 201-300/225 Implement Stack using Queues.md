# 225. Implement Stack using Queues(E)
[225. Implement Stack using Queues](https://leetcode-cn.com/problems/implement-stack-using-queues/)

## 题目描述(简单)

Implement the following operations of a stack using queues.

- push(x) -- Push element x onto stack.
- pop() -- Removes the element on top of the stack.
- top() -- Get the top element.
- empty() -- Return whether the stack is empty.

Example:
```
MyStack stack = new MyStack();

stack.push(1);
stack.push(2);  
stack.top();   // returns 2
stack.pop();   // returns 2
stack.empty(); // returns false
```
**Notes**:

- You must use only standard operations of a queue -- which means only push to back, peek/pop from front, size, and is empty operations are valid.
- Depending on your language, queue may not be supported natively. You may simulate a queue by using a list or deque (double-ended queue), as long as you use only standard operations of a queue.
- You may assume that all operations are valid (for example, no pop or top operations will be called on an empty stack).

## 思路

push:入队后之前的已入队的出队重新入队，实现后入先出
pop:队首出队
top:队首元素
empty:队列是否为空

## 解决方法

### 

```java 
public class MyStack {

    private Queue<Integer> queue;
    /** Initialize your data structure here. */
    public MyStack() {
        queue = new LinkedList<>();
    }
    
    /** Push element x onto stack. */
    public void push(int x) {
        queue.offer(x);
        int size = queue.size();
        while(size-->1){
        	queue.offer(queue.poll());
        }
    }
    
    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        return queue.poll();
        
    }
    
    /** Get the top element. */
    public int top() {
        return queue.peek();
    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        return queue.isEmpty();
    }
}
```

- 入栈push
```java
    public void push(int x) {
        queue.offer(x);
        int size = queue.size();
        while(size-->1){
        	queue.offer(queue.poll());
        }
    }
```
时间复杂度：O(n)算法会让 q1 出队 n 个元素，同时入队 n + 1 个元素到 q2。这个过程会产生 2n + 1 步操作，同时链表中 插入 操作和 移除 操作的时间复杂度为O(1)，因此时间复杂度为 O(n)。
空间复杂度：O(1)

- 出栈
```java
public int pop() {
return queue.poll();
}
```
时间复杂度：O(1)
空间复杂度：O(1)

- 取栈顶元素
```java
public int top() {
return queue.peek();
}
```
时间复杂度：O(1)
空间复杂度：O(1)

- 判空
```java
    public boolean empty() {
        return queue.isEmpty();
    }
```
时间复杂度：O(1)
空间复杂度：O(1)











