# 307. Range Sum Query - Mutable(M)

[307. 区域和检索 - 数组可修改](https://leetcode-cn.com/problems/range-sum-query-mutable/)

## 题目描述(中等)

给定一个整数数组  `nums`，求出数组从索引 i 到 j  (i ≤ j) 范围内元素的总和，包含 i,  j 两点。

`update(i, val)` 函数可以通过将下标为 i 的数值更新为 val，从而对数列进行修改。

示例:
```
Given nums = [1, 3, 5]

sumRange(0, 2) -> 9
update(1, 2)
sumRange(0, 2) -> 8
```
**说明**:

1. 数组仅可以在 update 函数下进行修改。
2. 你可以假设 update 函数与 sumRange 函数的调用次数是均匀分布的。


## 思路

- 调用即求和
- 缓存+更新
- 线段树


## 解决方法

### 

```java
public class NumArray1 {
    int[] nums;
    public NumArray1(int[] nums) {
        this.nums = Arrays.copyOf(nums, nums.length);
    }

    public void update(int i, int val) {
        nums[i] = val;
    }
    public int sumRange(int i, int j) {
        int sum = 0;
        for (int k = i; k <= j; k++) {
            sum += nums[k];
        }
        return sum;
    }
}
```

时间复杂度为 O(n)。更新查询的时间复杂度为 O(1)。
空间复杂度：O(1)

### 缓存+更新

修改元素时即更新缓存

```java
public class NumArray1_1 {

    int[] sums;
    int[] nums;

    public NumArray1_1(int[] nums) {
        this.nums = Arrays.copyOf(nums, nums.length);
        sums = new int[nums.length + 1];
        for (int i = 1; i <= nums.length; i++) {
            sums[i] += sums[i - 1] + nums[i - 1];
        }
    }

    public void update(int i, int val) {
        int diff = nums[i] - val;
        nums[i] = val;
        for (int k = i + 1; k < sums.length; k++) {
            sums[k] -= diff;
        }
    }

    public int sumRange(int i, int j) {
        return sums[j + 1] - sums[i];
    }
}

```

修改元素后延迟缓存，在调用区间求和时若有更新则更新缓存

```java
public class NumArray1_2 {

    int[] sums;
    int[] nums;
    boolean update;

    public NumArray1_2(int[] nums) {
        this.nums = nums;
        sums = new int[nums.length + 1];
        update = true;
    }

    public void update(int i, int val) {
        nums[i] = val;
        update = true;
    }

    public int sumRange(int i, int j) {
        if (!update) {
            return sums[j + 1] - sums[i];
        }
        sums = new int[nums.length + 1];
        for (int k = 1; k <= nums.length; k++) {
            sums[k] += sums[k - 1] + nums[k - 1];
        }
        update = false;
        return sums[j + 1] - sums[i];
    }
}

```

### 线段树

线段树是一种非常灵活的数据结构，它可以用于解决多种范围查询问题，比如在对数时间内从数组中找到最小值、最大值、总和、最大公约数、最小公倍数等。

数组 `A[0,1,…,n−1]` 的线段树是一个二叉树，其中每个节点都包含数组的一个子范围 `[i,j]` 上的聚合信息（最小值、最大值、总和等），其左、右子节点分别包含范围 `[i,(i+j)/2]和[(i+j/2+1,j)]`上的信息。

线段树既可以用数组也可以用树来实现。对于数组实现，如果索引 i 处的元素不是一个叶节点，那么其左子节点和右子节点分别存储在索引为 2i 和 2i+1 的元素处。

线段树可以分为以下三个步骤：
1. 从给定数组构建线段树的预处理步骤。
2. 修改元素时更新线段树。
3. 使用线段树进行区域和检索。
```
示例A=[2,4,5,7,8,9]

             35
          /     \
         29       6
       /   \     / \
      12    17  2   4
     / \   / \
    5   7 8   9
```

**构建线段树**

使用一种非常有效的自下而上的方法来构建线段树。从上面我们已经知道，如果某个节点 p 包含范围 `[i,j]` 的和，那么其左、右子节点分别包含范围 `[i,(i+j)/2]和[(i+j/2+1,j)]` 上的和 

为了找到节点 p 的和，需要提前计算其左、右子节点的和。

从叶节点开始，用输入数组的元素 `a[0,1,...,n-1]`初始化它们。然后我们逐步向上移动到更高一层来计算父节点的和，直到最后到达线段树的根节点

- 具有 n 个元素的数组线段树有 n 个叶节点（数组元素本身）。每一层中的节点数是下面一层中节点数的一半。
- 按层对节点数求和: $n+n/2+n/4+n/8+...+1≈2n$

- 当叶子结点即数组元素n=2^(h-1)时，树高h，总结点为2^h-1，数组线段树有效节点为`T[1,2n)`
- 当叶子结点即数组元素n!=2^(h-1)时，树高h=log(2n)的上取整，最后一层的叶节点个数为`2n-1-2^((h-1)-1)=2n-2^(h-1)`,数组线段树有效节点为`T[1,2n)`

**更新线段树**

更新数组中某个索引 i 处的元素时，我们需要重建线段树，因为一些树节点上的和值也会随之产生变化。将再次使用自下而上的方法。首先更新存储 a[i] 元素的叶节点。从那里我们将一路向上，直到根节点，并用其子节点值的总和来更新每个父节点的值

向上寻找根节点的过程中，其兄弟节点找法
- 若当前节点为左节点，即索引为偶数，兄弟节点为右节点
- 若当前节点为右节点，即索引为奇数，兄弟节点为左节点
- 归一为索引最低位的异或

时间复杂度：O(logn)
空间复杂度：O(1)

**区域检索**

可以通过以下方式使用线段树进行区域和检索 `[L, R]`：算法保持循环不变：`l≤r` 以及已经算出 `[L,l]`[L…l] 和 `[r,R]` 的总和，其中 l 和 r 分别是计算总和时的左边界和右边界。每次迭代的范围 `[l,r]`都会缩小，直到在算法的大约 logn 次迭代后两个边界相遇为止。

- 若l是右节点，其父节点为子节点之和，则求和时单独计算当前节点
- 若r是左节点，其父节点为子节点之和，则求和时单独计算当前节点
  
时间复杂度：O(logn)
空间复杂度：O(1)

```Java
public class NumArray1_3 {

    int[] sumTree;
    int[] nums;
    int size;

    public NumArray1_3(int[] nums) {
        this.nums = Arrays.copyOf(nums, nums.length);
        size = nums.length;
        sumTree = new int[size << 1];
        buildTree();
    }

    public void buildTree() {
        for (int i = size, j = 0; i < size << 1; j++, i++) {
            sumTree[i] = nums[j];
        }
        for (int i = size - 1; i >= 0; i--) {
            sumTree[i] = sumTree[i * 2] + sumTree[i * 2 + 1];
        }
    }

    public void update(int i, int val) {
        int pos = i;
        nums[pos] = val;
        pos += size;
        sumTree[pos] = val;
        while (pos > 1) {
            sumTree[pos >> 1] = sumTree[pos] + sumTree[pos ^ 1];
            pos >>= 1;
        }

    }

    public int sumRange(int i, int j) {
        int m = i + size;
        int n = j + size;
        int sum = 0;
        while (m <= n) {
            if ((m & 1) == 1) {
                sum += sumTree[m++];
            }
            m >>= 1;
            if ((n & 1) == 0) {
                sum += sumTree[n--];
            }
            n >>= 1;
        }
        return sum;
    }
}

```