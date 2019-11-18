# 284. Peeking Iterator(M)

[284. 顶端迭代器](https://leetcode-cn.com/problems/peeking-iterator/)

## 题目描述(中等)

Given an Iterator class interface with methods: `next()` and `hasNext()`, design and implement a PeekingIterator that support the `peek()` operation -- it essentially peek() at the element that will be returned by the next call to next().

Example:
```
Assume that the iterator is initialized to the beginning of the list: [1,2,3].

Call next() gets you 1, the first element in the list.

Now you call peek() and it returns 2, the next element. Calling next() after that still return 2. 

You call next() the final time and it returns 3, the last element. 

Calling hasNext() after that should return false.
```

**Follow up**: How would you extend your design to be generic and work with all types, not just integer?


## 思路

- 内部存储迭代器
- 底层数组实现

## 解决方法

### 内部存储迭代器

提前迭代一步，变量记录当前头部值

```java
public class PeekingIterator implements Iterator<Integer> {

    private Iterator<Integer> iterator;
    private Integer top = null;


    public PeekingIterator(Iterator<Integer> iterator) {
        // initialize any member here.
        this.iterator = iterator;
        if (iterator.hasNext()) {
            top = iterator.next();
        }
    }

    // Returns the next element in the iteration without advancing the iterator.
    public Integer peek() {
        return top;
    }

    // hasNext() and next() should behave the same as in the Iterator interface.
    // Override them if needed.
    @Override
    public Integer next() {
        Integer r = top;
        if (iterator.hasNext()) {
            top = iterator.next();
        } else {
            top = null;
        }
        return r;
    }

    @Override
    public boolean hasNext() {
        return top != null;
    }
}

```

### 数组实现

- peek：直接返回头指针的元素
- next：头指针后移,返回头指针的元素
- hasNext：头指针是否和尾指针重叠
- grow函数中每次容量扩大两倍,同时判断扩容时是否移除

```java
public class PeekingIterator_1 implements Iterator<Integer> {

    Integer[] elementData;
    int capacity;
    int size;
    int top = 0;

    public PeekingIterator_1(Iterator<Integer> iterator) {
        // initialize any member here.
        capacity = 2 << 4;
        elementData = new Integer[capacity];
        size = 0;
        while (iterator.hasNext()) {
            elementData[size++] = iterator.next();
            if (size == capacity) {
                grow();
            }
        }


    }

    // Returns the next element in the iteration without advancing the iterator.
    public Integer peek() {
        return elementData[top];
    }

    // hasNext() and next() should behave the same as in the Iterator interface.
    // Override them if needed.
    @Override
    public Integer next() {
        return elementData[top++];
    }

    @Override
    public boolean hasNext() {
        return top != size;
    }

    private void grow() {
        capacity = capacity << 2;
        elementData = Arrays.copyOf(elementData, capacity);
    }
}

```