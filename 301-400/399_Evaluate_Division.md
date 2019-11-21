# 399. Evaluate Division(M)

[399. 除法求值](https://leetcode-cn.com/problems/evaluate-division/)

## 题目描述(中等)

给出方程式 `A / B = k`, 其中 A 和 B 均为代表字符串的变量， k 是一个浮点型数字。根据已知方程式求解问题，并返回计算结果。如果结果不存在，则返回 -1.0。

示例 :
```
给定 a / b = 2.0, b / c = 3.0
问题: a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ? 
返回 [6.0, 0.5, -1.0, 1.0, -1.0 ]

输入为: vector<pair<string, string>> equations, vector<double>& values, vector<pair<string, string>> queries(方程式，方程式结果，问题方程式)， 其中 equations.size() == values.size()，即方程式的长度与方程式结果长度相等（程式与结果一一对应），并且结果值均为正数。以上为方程式的描述。 返回vector<double>类型。

基于上述例子，输入如下：

equations(方程式) = [ ["a", "b"], ["b", "c"] ],
values(方程式结果) = [2.0, 3.0],
queries(问题方程式) = [ ["a", "c"], ["b", "a"], ["a", "e"], ["a", "a"], ["x", "x"] ]. 
```

输入总是有效的。你可以假设除法运算中不会出现除数为0的情况，且不存在任何矛盾的结果。

## 思路

构建图
- DFS遍历
- BFS遍历
- Floyd
- 并查集

## 解决方法

### DFS遍历

```java
    public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {
        Map<String, Map<String, Double>> graph = buildGraph(equations, values);
        double[] result = new double[queries.size()];
        for (int i = 0; i < queries.size(); i++) {
            Double r = dfsQuery(graph, queries.get(i).get(0), queries.get(i).get(1), new HashSet<>());
            result[i] = r != null ? r : -1.0;
        }
        return result;
    }

    public Map<String, Map<String, Double>> buildGraph(List<List<String>> equations, double[] values) {
        Map<String, Map<String, Double>> graph = new HashMap<>();
        int n = equations.size();
        for (int i = 0; i < n; i++) {
            String x = equations.get(i).get(0);
            String y = equations.get(i).get(1);
            double value = values[i];
            if (!graph.containsKey(x)) {
                graph.put(x, new HashMap<>());
            }
            graph.get(x).put(y, value);
            if (!graph.containsKey(y)) {
                graph.put(y, new HashMap<>());
            }
            graph.get(y).put(x, 1 / value);
        }
        return graph;
    }

    public Double dfsQuery(Map<String, Map<String, Double>> graph, String a, String b, Set<String> visited) {
        if (!graph.containsKey(a)) {
            return null;
        }
        Map<String, Double> adjMap = graph.get(a);
        if (adjMap.containsKey(b)) {
            return graph.get(a).get(b);
        }
        visited.add(a);
        for (String adj : adjMap.keySet()) {
            if (visited.contains(adj)) {
                continue;
            }
            Double r = dfsQuery(graph, adj, b, visited);
            if (r != null) {
                return adjMap.get(adj) * r;
            }
        }
        visited.remove(a);
        return null;
    }
```

### BFS 

```java
    public double[] calcEquation1(List<List<String>> equations, double[] values, List<List<String>> queries) {
        Map<String, Map<String, Double>> graph = buildGraph(equations, values);
        double[] result = new double[queries.size()];
        for (int i = 0; i < queries.size(); i++) {
            Double r = bfsQuery(graph, queries.get(i).get(0), queries.get(i).get(1));
            result[i] = r != null ? r : -1.0;
        }
        return result;
    }

    public Double bfsQuery(Map<String, Map<String, Double>> graph, String a, String b) {
        if (!graph.containsKey(a)) {
            return null;
        }
        Queue<Pair<String, Double>> queue = new LinkedList<>();
        queue.add(new Pair<>(a, 1.0));
        Set<String> visited = new HashSet<>();
        visited.add(a);
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                Pair<String, Double> pair = queue.poll();
                if (b.equals(pair.getKey())) {
                    return pair.getValue();
                }
                Map<String, Double> adjMap = graph.get(pair.getKey());
                for (String adj : adjMap.keySet()) {
                    if (visited.contains(adj)) {
                        continue;
                    }
                    queue.add(new Pair<>(adj, pair.getValue() * adjMap.get(adj)));
                    visited.add(adj);
                }
            }
        }
        return null;
    }
```

### Floyd

```java
    public double[] calcEquation2(List<List<String>> equations, double[] values, List<List<String>> queries) {
        Map<String, Map<String, Double>> graph = buildGraph(equations, values);
        double[] result = new double[queries.size()];
        Map<String, Integer> indexMap = new HashMap<>();
        int index = 0;
        for (String node : graph.keySet()) {
            indexMap.put(node, index++);
        }
        Double[][] floydValues = floyd(graph, indexMap);
        for (int i = 0; i < queries.size(); i++) {
            String x = queries.get(i).get(0);
            String y = queries.get(i).get(1);
            if (indexMap.containsKey(x) && indexMap.containsKey(y)) {
                Double f = floydValues[indexMap.get(x)][indexMap.get(y)];
                result[i] = f != null ? f : -1.0;
            } else {
                result[i] = -1.0;
            }
        }
        return result;
    }

    public Double[][] floyd(Map<String, Map<String, Double>> graph, Map<String, Integer> indexMap) {
        int n = graph.size();
        Double[][] values = new Double[n][n];
        for (String a : graph.keySet()) {
            for (String b : graph.get(a).keySet()) {
                values[indexMap.get(a)][indexMap.get(b)] = graph.get(a).get(b);
            }
        }
        for (int i = 0; i < n; i++) {
            values[i][i] = 1.0;
            for (int j = 0; j < n; j++) {
                for (int k = 0; k < n; k++) {
                    if (values[i][j] == null && values[i][k] != null && values[k][j] != null) {
                        values[i][j] = values[i][k] * values[k][j];
                    }
                }
            }
        }
        return values;
    }

```
### 并查集

