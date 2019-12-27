
# 528. Random Pick with Weight(M)

[528. 按权重随机选择](https://leetcode-cn.com/problems/random-pick-with-weight/)

## 题目描述()

给定一个正整数数组 w ，其中 w[i] 代表位置 i 的权重，请写一个函数 pickIndex ，它可以随机地获取位置 i，选取位置 i 的概率与 w[i] 成正比。

**说明**:
- 1 <= w.length <= 10000
- 1 <= w[i] <= 10^5
- pickIndex 将被调用不超过 10000 次

示例1:
```
输入: 
["Solution","pickIndex"]
[[[1]],[]]
输出: [null,0]
```

示例2:
```
输入: 
["Solution","pickIndex","pickIndex","pickIndex","pickIndex","pickIndex"]
[[[1,3]],[],[],[],[],[]]
输出: [null,0,1,1,1,0]
```

**输入语法说明**：
- 输入是两个列表：调用成员函数名和调用的参数。Solution 的构造函数有一个参数，即数组 w。pickIndex 没有参数。输入参数是一个列表，即使参数为空，也会输入一个 [] 空列表。


## 思路

前缀和表示权重的累计

随机数处于哪两个累计权重之间，就说明属于前者或后者代表的值

## 解决方法

### 二分查找

sumWeight[i] 存储第i个数前的累计权重(包括第i个)，随机生成一个[0,sum)之间的数字target，利用二分查找找到第一个大于target的索引，

第i个数所包括的权重范围为[sumWeight[i-1],sumWeight[i])


求出前缀和数组 p，其中 $p[x] = \sum_{i=0}^{x}w[i]$

在范围 [0,tot) 中随机选择一个整数 target。

使用二分查找来找到下标 x，其中 x 是满足 target < p[x] 的最小下标。

对于某个下标 i，所有满足 p[i]−w[i] ≤ v < p[i] 的整数 v 都映射到这个下标。因此，所有的下标都与下标权重成比例。


```java
public class S528RandomPickWithWeight {

    int[] sumWeight;
    Random random;

    public S528RandomPickWithWeight(int[] w) {
        sumWeight = new int[w.length];
        random = new Random();
        int sum = 0;
        for (int i = 0; i < w.length; i++) {
            sum += w[i];
            sumWeight[i] = sum;
        }
    }

    public int pickIndex() {
        int target = random.nextInt(sumWeight[sumWeight.length - 1]);
        int left = 0, right = sumWeight.length - 1;
        while (left < right) {
            int mid = (left + right) >>> 1;
            if (sumWeight[mid] <= target) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        return left;
    }
}
```

### TreeMap

采用TreeMap存储累计权重和索引的关系，key为第i个数前的累计权重(不包括第i个)，随机生成一个[0,sum)之间的数字target，利用TreeMap的有序性找到小于等于target的索引

第i个数所包括的权重范围为 sumWeight[i-1]所对应的索引i

```java
public class S528RandomPickWithWeight_1 {

    TreeMap<Integer, Integer> weight;
    Random random;
    int sum;

    public S528RandomPickWithWeight_1(int[] w) {
        weight = new TreeMap<>();
        random = new Random();
        sum = 0;
        for (int i = 0; i < w.length; i++) {
            weight.put(sum, i);
            sum += w[i];
        }
    }

    public int pickIndex() {
        int target = random.nextInt(sum);
        int index = weight.get(weight.floorKey(target));
        return index;
    }
}

```