# 216. Combination Sum III(M)

[216. 组合总和 III](https://leetcode-cn.com/problems/combination-sum-iii/)

## 题目描述(中等)

Find all possible combinations of **k** numbers that add up to a number **n**, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.

**Note**:

- All numbers will be positive integers.
- The solution set must not contain duplicate combinations.

Example 1:
```
Input: k = 3, n = 7
Output: [[1,2,4]]
```
Example 2:
```
Input: k = 3, n = 9
Output: [[1,2,6], [1,3,5], [2,3,4]]
```


## 思路

- 回溯

## 解决方法

### 回溯

递归终止条件：数组中包含k个数，如果和为n则加入结果集，否则直接返回终止递归
递归过程：循环遍历1-9，将新数字加入临时数组中进入下一层递归，出来后再将其移除

```java
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> listList = new ArrayList<>();
        combine(listList, new ArrayList<>(), k, 1, n);
        return listList;
    }

    public void combine(List<List<Integer>> listList, List<Integer> list, int k, int index, int remain) {
        if (list.size() == k) {
            if (remain == 0) {
                listList.add(new ArrayList<>(list));
            }
            return;
        }
        if (remain < 0) {
            return;
        }
        for (int i = index; i <= 9; i++) {
            list.add(i);
            combine(listList, list, k, i + 1, remain - i);
            list.remove(list.size() - 1);
        }
    }

```