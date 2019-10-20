# 060. Permutation Sequence(M)
[060. 第k个排列](https://leetcode-cn.com/problems/permutation-sequence/)


## 题目描述(中等)

The set [1,2,3,...,n] contains a total of n! unique permutations.

By listing and labeling all of the permutations in order, we get the following sequence for n = 3:
```
1. "123"
2. "132"
3. "213"
4. "231"
5. "312"
6. "321"
```
Given n and k, return the $$ k^{th} $$ permutation sequence.

**Note**:
- Given n will be between 1 and 9 inclusive.
- Given k will be between 1 and n! inclusive.

Example 1:
```
Input: n = 3, k = 3
Output: "213"
```
Example 2:
```
Input: n = 4, k = 9
Output: "2314"
```

## 思路

因为是从小到大排列，那么最高位一定是从小到大，从1到n，每个数开头的看成一组，依次求出k属于哪组


## 解决方法


### 
```java
public String getPermutation(int n, int k) {
        List<Integer> list = new ArrayList<>();
        StringBuilder sb = new StringBuilder();
        for (int i = 1; i <= n; i++) {
            list.add(i);
        }
        int num = fabric(n);
        while (n > 0) {
            num /= n--;
            int groupNum = k / num;
            k %= num;
            if (k == 0) {
                sb.append(list.get(groupNum - 1));
                list.remove(groupNum - 1);
                break;
            } else {
                sb.append(list.get(groupNum));
                list.remove(groupNum);
            }
        }
        //Collections.reverse(list);
        for (int i = list.size() - 1; i >= 0; i--) {
            sb.append(list.get(i));
        }
        return sb.toString();
    }
    private int fabric(int n) {
        int fab = 1;
        while (n != 0) {
            fab *= n--;
        }
        return fab;
    }

```

```java
    public String getPermutation1(int n, int k) {
        List<Integer> list = new ArrayList<>();
        StringBuilder sb = new StringBuilder();
        for (int i = 1; i <= n; i++) {
            list.add(i);
        }
        int num = fabric(n);
        k--;
        while (n > 0) {
            num /= n--;
            int groupNum = k / num;
            k %= num;
            sb.append(list.get(groupNum));
            list.remove(groupNum);
        }
        return sb.toString();
    }

    private int fabric(int n) {
        int fab = 1;
        while (n != 0) {
            fab *= n--;
        }
        return fab;
    }
```

时间复杂度：O(n)。

空间复杂度：O(1)。
