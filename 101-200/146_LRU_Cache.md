# 146. LRU Cache\(M\)

[146. LRU缓存机制](https://leetcode-cn.com/problems/lru-cache/)

## 题目描述\(中等\)

Design and implement a data structure for **Least Recently Used \(LRU\) cache**. It should support the following operations: get and put.

**get\(key\)** - Get the value \(will always be positive\) of the key if the key exists in the cache, otherwise return -1.  
**put\(key, value\)** - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

The cache is initialized with a **positive **capacity.

**Follow up**:  
Could you do both operations in **O\(1\)** time complexity?

Example:

```
LRUCache cache = new LRUCache( 2 /* capacity */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.put(4, 4);    // evicts key 1
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
```

## 思路

* 字典树
* 哈希+双向链表

## 解决方法

### 哈希+双向链表

![](../assets/leetcode-note/101-200/146-s-2-1.png)

借助哈希表赋予了链表快速查找的特性嘛：可以快速查找某个 key 是否存在缓存（链表）中，同时可以快速删除、添加节点。

- 为什么要是双向链表，单链表行不行？
    - 单向链表无法实现O(1)的时间复杂度，在插入和删除时是在表头表尾处进行，单向链表对表尾无法满足，删除一个节点不光要得到该节点本身的指针，也需要操作其前驱节点的指针，而双向链表才能支持直接查找前驱，保证操作的时间复杂度 O(1)O(1)。
- 另外，既然哈希表中已经存了 key，为什么链表中还要存键值对呢，只存值不就行了？
    - 在容量满的时候，删除cache中需要根据key，获取链表尾部节点，需要从节点中获取key

```java
public class LRUCache {

    DoubleListNode head, tail;
    Map<Integer, DoubleListNode> cache;
    int capacity;
    int size;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        size = 0;
        cache = new HashMap<>();
        head = new DoubleListNode(-1, -1);
        tail = new DoubleListNode(-1, -1);
        head.next = tail;
        tail.prev = head;
    }

    public int get(int key) {
        if (size == 0) {
            return -1;
        }
        if (cache.containsKey(key)) {
            DoubleListNode node = cache.get(key);
            moveToHead(node);
            return node.value;
        }
        return -1;
    }

    public void put(int key, int value) {
        if (capacity <= 0) {
            return;
        }
        DoubleListNode node = new DoubleListNode(key, value);
        if (cache.containsKey(key)) {
            removeNode(cache.get(key));
        } else {
            if (size == capacity) {
                DoubleListNode last = removeLast();
                cache.remove(last.key);
            }
        }
        addNode(node);
        cache.put(key, node);
    }

    public void addNode(DoubleListNode node) {
        node.next = head.next;
        node.prev = head;
        head.next.prev = node;
        head.next = node;
        size++;
    }

    public void removeNode(DoubleListNode node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
        size--;
    }

    public void moveToHead(DoubleListNode node) {
        removeNode(node);
        addNode(node);
    }

    public DoubleListNode removeLast() {
        if (tail.prev == null) {
            return null;
        }
        DoubleListNode last = tail.prev;
        removeNode(last);
        return last;
    }

    class DoubleListNode {
        public int key;
        public int value;
        public DoubleListNode prev;
        public DoubleListNode next;

        public DoubleListNode(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }
}
```



