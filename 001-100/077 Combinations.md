# 077. Combinations(M)
[077. 组合](https://leetcode-cn.com/problems/combinations/)


## 题目描述(中等)

Given two integers n and k, return all possible combinations of k numbers out of 1 ... n.

Example:
```
Input: n = 4, k = 2
Output:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

## 思路

回溯

## 解决方法


### 回溯

- 若组合完成- 添加到输出中。

- 遍历从 first t到 n的所有整数。

    - 将整数 i 添加到现有组合 curr中。

    - 继续向组合中添加更多整数 :backtrack(i + 1, curr).

    - 将 i 从 curr中移除，实现回溯。



```java
    public List<List<Integer>> combine0(int n, int k) {
        List<List<Integer>> listList = new ArrayList<>();
        if (n <= 0 || k <= 0 || k > n) {
            return listList;
        }
        backtrack(listList, new ArrayList<>(), n, k, 1);
        return listList;
    }

    private void backtrack(List<List<Integer>> listList, List<Integer> list, int n, int k, int index) {
        if (list.size() == k) {
            listList.add(new ArrayList<>(list));
            return;
        }
        for (int i = index + 1; i <= n; i++) {
            list.add(i);
            backtrack(listList, list, n, k, i);
            list.remove(list.size() - 1);
        }
    }
```

### 回溯+剪枝

修改for循环结束条件，剩余未选的数字全选若不满足总数要求，不需要再循环。

比如，n = 5，k = 4，list.size( ) == 1，此时代表我们还需要（4 - 1 = 3）个数字，如果 i = 4 的话，以后最多把 4 和 5 加入到 list中，而此时 list.size() 才等于 1 + 2 = 3，不够 4 个，所以 i 没必要等于 4，i 循环到 3 就足够了。

```
    public List<List<Integer>> combine0(int n, int k) {
        List<List<Integer>> listList = new ArrayList<>();
        if (n <= 0 || k <= 0 || k > n) {
            return listList;
        }
        backtrack1(listList, new ArrayList<>(), n, k, 1);
        return listList;
    }
    private void backtrack1(List<List<Integer>> listList, List<Integer> list, int n, int k, int index) {
        if (list.size() == k) {
            listList.add(new ArrayList<>(list));
            return;
        }
        for (int i = index; i <= n - (k - list.size()) + 1; i++) {
            list.add(i);
            backtrack1(listList, list, n, k, i + 1);
            list.remove(list.size() - 1);
        }
    }
```




### 回溯迭代

回溯其实有三个过程。

- for 循环结束，也就是 i == n + 1，然后回到上一层的 for 循环
- temp.size() == k，也就是所需要的数字够了，然后把它加入到结果中。
- 每个 for 循环里边，进入递归，添加下一个数字


```java
    public List<List<Integer>> combine1(int n, int k) {
        List<List<Integer>> listList = new ArrayList<>();
        List<Integer> temp = new ArrayList<>();
        for (int i = 0; i < k; i++) {
            temp.add(0);
        }
        int i = 0;
        while (i >= 0) {
            //当前数字加 1
            temp.set(i, temp.get(i) + 1);
            //当前数字大于 n，对应回溯法的 i == n + 1，然后回到上一层
            //优化为剩余未选数量不够，回溯
            if (temp.get(i) > n - (k - i - 1)) {
                i--;
            } else if (i == k - 1) {
                // 当前数字个数够了
                listList.add(new ArrayList<>(temp));
                //进入更新下一个数字
            } else {
                i++;
                //把下一个数字置为上一个数字，类似于回溯法中的 start
                temp.set(i, temp.get(i - 1));
            }
        }
        return listList;
    }
```



