# 133. Clone Graph\(M\)
[133. 克隆图](https://leetcode-cn.com/problems/clone-graph/)


## 题目描述\(中等\)

Given a reference of a node in a **connected **undirected graph, return a **deep copy** \(clone\) of the graph. Each node in the graph contains a val `(int)` and a list `(List[Node])` of its neighbors.

Example:

![](/assets/101-200/133-p-1.png)

图的节点定义
```java
class Node {
    public int val;
    public List<Node> neighbors;

    public Node() {
    }

    public Node(int _val, List<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
```

## 思路

- BFS
- DFS

## 解决方法

### BFS

首先对图进行一个 BFS，把所有节点 new 出来，不处理 neighbors ，并且把所有的节点存到 map 中。

然后再对图做一个 BFS，因为此时所有的节点已经创建了，只需要更新所有节点的 neighbors。

```java
    public Node cloneGraph(Node node) {
        if (node == null) {
            return null;
        }
        Map<Node, Node> map = new HashMap<>();
        Node newNode = new Node(node.val, new ArrayList<>());
        map.put(node, newNode);
        Queue<Node> queue = new LinkedList<>();
        queue.add(node);
        while (!queue.isEmpty()) {
            Node curNode = queue.poll();
            for (Node neighbor : curNode.neighbors) {
                if (!map.containsKey(neighbor)) {
                    queue.add(neighbor);
                    Node n = new Node(neighbor.val, new ArrayList<>());
                    map.put(neighbor, n);
                }
                map.get(curNode).neighbors.add(map.get(neighbor));
            }
        }
        return newNode;
    }
```

### DFS

```java
    public Node cloneGraph1(Node node) {
        if (node == null) {
            return node;
        }
        Map<Node, Node> map = new HashMap<>();
        return cloneGraph1(node, map);
    }

    public Node cloneGraph1(Node node, Map<Node, Node> map) {
        if (map.containsKey(node)) {
            return map.get(node);
        }
        Node n = new Node(node.val, new ArrayList<>());
        map.put(node, n);
        for (Node neighbor : node.neighbors) {
            n.neighbors.add(cloneGraph1(neighbor, map));
        }
        return n;
    }
```


