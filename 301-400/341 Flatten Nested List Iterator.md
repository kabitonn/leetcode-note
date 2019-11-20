
# 341. Flatten Nested List Iterator(M)

[341. 扁平化嵌套列表迭代器](https://leetcode-cn.com/problems/flatten-nested-list-iterator/)

## 题目描述(中等)

给定一个嵌套的整型列表。设计一个迭代器，使其能够遍历这个整型列表中的所有整数。

列表中的项或者为一个整数，或者是另一个列表。

示例 1:
```
输入: [[1,1],2,[1,1]]
输出: [1,1,2,1,1]
解释: 通过重复调用 next 直到 hasNext 返回false，next 返回的元素的顺序应该是: [1,1,2,1,1]。
```
示例 2:
```
输入: [1,[4,[6]]]
输出: [1,4,6]
解释: 通过重复调用 next 直到 hasNext 返回false，next 返回的元素的顺序应该是: [1,4,6]。
```


## 思路

- DFS遍历
- 辅助栈

## 解决方法

### DFS 遍历

在初始化迭代器的时候就直接把结果遍历出来，递归遍历列表中的数据，是整数就放入List，不是则再递归遍历

```java
    public class NestedIterator implements Iterator<Integer> {

    Queue<Integer> queue;

    public NestedIterator(List<NestedInteger> nestedList) {
        queue = new LinkedList<>();
        dfs(nestedList);
    }

    private void dfs(List<NestedInteger> nestedList) {
        for (NestedInteger nested : nestedList) {
            if (nested.isInteger()) {
                queue.add(nested.getInteger());
            } else {
                dfs(nested.getList());
            }
        }
    }

    @Override
    public Integer next() {
        return queue.poll();
    }

    @Override
    public boolean hasNext() {
        return !queue.isEmpty();
    }
}
```

```java
    public class NestedIterator_1 implements Iterator<Integer> {

    Integer[] elementData;
    int capacity;
    int size;
    int top;

    public NestedIterator_1(List<NestedInteger> nestedList) {
        capacity = 2 << 4;
        elementData = new Integer[capacity];
        dfs(nestedList);
        top = 0;
    }

    private void dfs(List<NestedInteger> nestedList) {
        for (NestedInteger nested : nestedList) {
            if (nested.isInteger()) {
                elementData[size++] = nested.getInteger();
                if (size == capacity) {
                    grow();
                }
            } else {
                dfs(nested.getList());
            }
        }
    }

    private void grow() {
        capacity = capacity << 2;
        elementData = Arrays.copyOf(elementData, capacity);
    }

    @Override
    public Integer next() {
        return elementData[top++];
    }

    @Override
    public boolean hasNext() {
        return top != size;
    }
}
```
### 辅助栈

栈顶元素作为当前活跃的元素

- 取出栈顶活跃的列表迭代器
  - 若迭代器为空则出栈
  - 取出迭代器下一元素，若为列表则入栈，否则跳出循环

```java
public class NestedIterator_2 implements Iterator<Integer> {

    LinkedList<Iterator<NestedInteger>> stack;
    Integer num = null;

    public NestedIterator_2(List<NestedInteger> nestedList) {
        stack = new LinkedList<>();
        stack.push(nestedList.iterator());
    }


    @Override
    public Integer next() {
        while (!stack.isEmpty() && num == null) {
            Iterator<NestedInteger> iterator = stack.peek();
            if (!iterator.hasNext()) {
                stack.pop();
            } else {
                NestedInteger next = iterator.next();
                if (next.isInteger()) {
                    num = next.getInteger();
                    iterator.remove();
                    break;
                } else {
                    stack.push(next.getList().iterator());
                    iterator.remove();
                }
            }
        }
        Integer tmp = num;
        num = null;
        return tmp;
    }

    @Override
    public boolean hasNext() {
        while (!stack.isEmpty()) {
            Iterator<NestedInteger> iterator = stack.peek();
            if (!iterator.hasNext()) {
                stack.pop();
            } else {
                NestedInteger next = iterator.next();
                if (next.isInteger()) {
                    num = next.getInteger();
                    iterator.remove();
                    break;
                } else {
                    stack.push(next.getList().iterator());
                    iterator.remove();
                }
            }
        }
        return num != null;
    }
}

```