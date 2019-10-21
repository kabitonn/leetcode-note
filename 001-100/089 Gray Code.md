# 089. Gray Code\(M\)

[089. 格雷编码](https://leetcode-cn.com/problems/gray-code/)

## 题目描述\(中等\)

The gray code is a binary numeral system where two successive values differ in only one bit.

Given a non-negative integer n representing the total number of bits in the code, print the sequence of gray code. A gray code sequence must begin with 0.

Example 1:

```
Input: 2
Output: [0,1,3,2]
Explanation:
00 - 0
01 - 1
11 - 3
10 - 2

For a given n, a gray code sequence may not be uniquely defined.
For example, [0,2,3,1] is also a valid gray code sequence.

00 - 0
10 - 2
11 - 3
01 - 1
```

Example 2:

```
Input: 0
Output: [0]
Explanation: We define the gray code sequence to begin with 0.
             A gray code sequence of n has size = 2n, which for n = 0 the size is 20 = 1.
             Therefore, for n = 0 the gray code sequence is [0].
```

## 思路

1. 遍历生成判断是否存在
2. 动态规划
3. 生成规律
4. 公式

## 解决方法

### 遍历生成

```java
    public List<Integer> grayCode(int n) {
        List<Integer> list = new ArrayList<>();
        int[] code = new int[n];
        for (int i = 0; i < n; i++) {
            code[i] = 1 << i;
        }
        int num = 1 << n;
        Set<Integer> set = new HashSet<>();
        list.add(0);
        set.add(0);
        while (list.size() != num) {
            int last = list.get(list.size() - 1);
            for (int i = 0; i < n; i++) {
                int cur = last ^ code[i];
                if (!set.contains(cur)) {
                    list.add(cur);
                    set.add(cur);
                    break;
                }
            }
        }
        return list;
    }
```

### 动态规划

假设我们有了 n = 2 的解，考虑怎么得到 n = 3 的解。

![](/assets/001-100/089-s-2-1.png)

```java
    public List<Integer> grayCode2(int n) {
        List<Integer> gray = new ArrayList<Integer>();
        //初始化 n = 0 的解
        gray.add(0);
        for (int i = 0; i < n; i++) {
            //要加的数
            int add = 1 << i;
            //倒序遍历，并且加上一个值添加到结果中
            for (int j = gray.size() - 1; j >= 0; j--) {
                gray.add(gray.get(j) + add);
            }
        }
        return gray;
    }
```

时间复杂度：$$ O(2^n) $$，因为有这么多的结果。

空间复杂度：O(1)。



