
# 382. Linked List Random Node(M)

[382. 链表随机节点](https://leetcode-cn.com/problems/linked-list-random-node/)

## 题目描述(中等)

给定一个单链表，随机选择链表的一个节点，并返回相应的节点值。保证每个节点**被选的概率一样**。

进阶:
如果链表十分大且长度未知，如何解决这个问题？你能否使用常数级空间复杂度实现？

示例:
```
// 初始化一个单链表 [1,2,3].
ListNode head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);
Solution solution = new Solution(head);

// getRandom()方法应随机返回1,2,3中的一个，保证每个元素被返回的概率相等。
solution.getRandom();
```

## 思路

- List存储
- 每次遍历

## 解决方法

### List

初始化时List存储所有链表节点

```java
public class S382LinkedListRandomNode {

    List<Integer> listNodes;
    Random random;

    public S382LinkedListRandomNode(ListNode head) {
        listNodes = new ArrayList<>();
        random = new Random();
        while (head != null) {
            listNodes.add(head.val);
            head = head.next;
        }
    }

    /**
     * Returns a random node's value.
     */
    public int getRandom() {
        int randomIndex = random.nextInt(listNodes.size());
        return listNodes.get(randomIndex);
    }
}

```

### 遍历

存储链表长度，随机获取索引，依次向后遍历节点

```java
public class S382LinkedListRandomNode_1 {

    ListNode head;
    int length;
    Random random;

    public S382LinkedListRandomNode_1(ListNode head) {
        this.head = head;
        random = new Random();
        while (head != null) {
            length++;
            head = head.next;
        }
    }

    /**
     * Returns a random node's value.
     */
    public int getRandom() {
        int randomIndex = random.nextInt(length);
        ListNode head = this.head;
        while (randomIndex-- > 0) {
            head = head.next;
        }
        return head.val;
    }
}

```
