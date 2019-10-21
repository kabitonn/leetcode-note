# 117. Populating Next Right Pointers in Each Node II\(M\)

## 题目描述\(中等\)

Given a binary tree

```
struct Node {  
  int val;  
  Node *left;  
  Node *right;  
  Node *next;  
}
```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

**Example**:

![](/assets/101-200/117-p-1.png)

## 思路

* 递归
* BFS
* 
## 解决方法

### 递归

```java
    public Node connect0(Node root) {
        if (root == null) {
            return root;
        }
        if (root.left != null || root.right != null) {
            if (root.left != null) {
                root.left.next = root.right;
            }
            Node pre = root.left;
            if (root.right != null) {
                pre = root.right;
            }
            Node next = root.next, conn = null;
            while (next != null && next.left == null && next.right == null) {
                next = next.next;
            }
            if (next != null) {
                if (next.left != null) {
                    conn = next.left;
                } else {
                    conn = next.right;
                }
            }
            pre.next = conn;
        }
        connect0(root.right);
        connect0(root.left);
        return root;
    }
```

```java
    //先构造右子树的next,否则父节点的兄弟节点会寻找出错
    public Node connect(Node root) {
        if (root == null) {
            return root;
        }
        if (root.left != null && root.right != null) {
            root.left.next = root.right;
            root.right.next = nextNode(root.next);
        } else if (root.left == null && root.right != null) {
            root.right.next = nextNode(root.next);
        } else if (root.left != null && root.right == null) {
            root.left.next = nextNode(root.next);
        }
        connect(root.right);
        connect(root.left);
        return root;
    }
    private Node nextNode(Node next) {
        Node conn = null;
        while (next != null) {
            if (next.left != null) {
                conn = next.left;
                break;
            }
            if (next.right != null) {
                conn = next.right;
                break;
            }
            next = next.next;
        }
        return conn;
    }
```
### BFS
```java
    public Node connect1(Node root) {
        if (root == null) {
            return root;
        }
        Queue<Node> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            Node pre = null;
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                Node p = queue.poll();
                if (pre != null) {
                    pre.next = p;
                }
                if (p.left != null) {
                    queue.add(p.left);
                }
                if (p.right != null) {
                    queue.add(p.right);
                }
                pre = p;
            }
        }
        return root;
    }
```


### 迭代

```java
    public Node connect2(Node root) {
        Node start = root;
        Node cur = null;
        while (start != null) {
            cur = start;
            while (cur != null) {
                //找到至少有一个孩子的节点
                if (cur.left == null && cur.right == null) {
                    cur = cur.next;
                    continue;
                }
                if (cur.left != null && cur.right != null) {
                    cur.left.next = cur.right;
                }
                Node pre = cur.right == null ? cur.left : cur.right;
                Node next = cur.next, conn = null;
                //找到当前节点的下一个至少有一个孩子的节点
                while (next != null && next.left == null && next.right == null) {
                    next = next.next;
                }
                if (next != null) {
                    if (next.left != null) {
                        conn = next.left;
                    } else {
                        conn = next.right;
                    }
                }
                pre.next = conn;
                cur = next;
            }
            //找到拥有孩子的节点
            while (start != null && start.left == null && start.right == null) {
                start = start.next;
            }
            if (start == null) {
                break;
            }
            //进入下一层
            start = start.left != null ? start.left : start.right;
        }
        return root;
    }
```

### dummy节点

用一个dummy指针，当连接第一个节点的时候，就将dummy指针指向他。此外，之前用的pre指针，把它当成tail指针可能会更好理解。

![](/assets/101-200/117-s-4-1.png)

cur 指针利用 next 不停的遍历当前层。

如果 cur 的孩子不为 null 就将它接到 tail 后边，然后更新tail。

当 cur 为 null 的时候，再利用 dummy 指针得到新的一层的开始节点。

dummy 指针在链表中经常用到，他只是为了处理头结点的情况，它并不属于当前链表。

```java
    public Node connect3(Node root) {
        Node cur = root;
        while (cur != null) {
            Node dummy = new Node();
            Node tail = dummy;
            //遍历 cur 的当前层
            while (cur != null) {
                if (cur.left != null) {
                    tail.next = cur.left;
                    tail = tail.next;
                }
                if (cur.right != null) {
                    tail.next = cur.right;
                    tail = tail.next;
                }
                cur = cur.next;
            }
            //更新 cur 到下一层
            cur = dummy.next;
        }
        return root;
    }
```



