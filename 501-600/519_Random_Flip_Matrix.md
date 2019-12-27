
# 519. Random Flip Matrix(M)

[519. 随机翻转矩阵](https://leetcode-cn.com/problems/random-flip-matrix/)

## 题目描述()

题中给出一个 n 行 n 列的二维矩阵 (n_rows,n_cols)，且所有值被初始化为 0。要求编写一个 flip 函数，均匀随机的将矩阵中的 0 变为 1，并返回该值的位置下标 [row_id,col_id]；同样编写一个 reset 函数，将所有的值都重新置为 0。尽量最少调用随机函数 Math.random()，并且优化时间和空间复杂度。

注意:
- 1 <= n_rows, n_cols <= 10000
- 0 <= row.id < n_rows 并且 0 <= col.id < n_cols
- 当矩阵中没有值为 0 时，不可以调用 flip 函数
- 调用 flip 和 reset 函数的次数加起来不会超过 1000 次

示例 1：
```
输入: 
["Solution","flip","flip","flip","flip"]
[[2,3],[],[],[],[]]
输出: [null,[0,1],[1,2],[1,0],[1,1]]
```

示例 2：
```
输入: 
["Solution","flip","flip","reset","flip"]
[[1,2],[],[],[],[]]
输出: [null,[0,0],[0,1],null,[0,0]]
```

**输入语法解释**：

输入包含两个列表：被调用的子程序和他们的参数。Solution 的构造函数有两个参数，分别为 n_rows 和 n_cols。flip 和 reset 没有参数，参数总会以列表形式给出，哪怕该列表为空



## 思路

首先坐标可映射为一维索引

- Set记录已翻转的坐标
- Map记录已翻转坐标的映射

## 解决方法

### 哈希集存储已翻转的坐标

随机生成索引直到索引不属于已翻转集合

```java
public class S519RandomFlipMatrix {

    Set<Integer> flippedIndex;
    Random random;
    int rows, cols, size;

    public S519RandomFlipMatrix(int n_rows, int n_cols) {
        random = new Random();
        rows = n_rows;
        cols = n_cols;
        size = rows * cols;
        flippedIndex = new HashSet<>();
    }

    public int[] flip() {
        if (flippedIndex.size() == size) {
            return null;
        }
        int index;
        do {
            index = random.nextInt(size);
        } while (flippedIndex.contains(index));
        int[] pos = {index / cols, index % cols};
        flippedIndex.add(index);
        return pos;
    }

    public void reset() {
        flippedIndex.clear();
    }
}
```

### 哈希表记录已翻转坐标的映射

黑名单到白名单的映射

记录未翻转坐标的个数，随机生成序号
- 若该序号已翻转过，则获取其重新映射后代表的索引，若改序号未翻转过，序号即为索引
- 哈希表记录已翻转的序号，并将该序号重新映射到尾部序号所代表的的索引，相当于和尾部序号代表元素进行交换，从而使得当前序号仍有映射的索引

```java
public class S519RandomFlipMatrix_1 {

    Map<Integer, Integer> flippedIndex;
    Random random;
    int rows, cols;
    int availableNum;


    public S519RandomFlipMatrix_1(int n_rows, int n_cols) {
        random = new Random();
        rows = n_rows;
        cols = n_cols;
        availableNum = rows * cols;
        flippedIndex = new HashMap<>();
    }

    public int[] flip() {
        if (availableNum == 0) {
            return null;
        }
        int i = random.nextInt(availableNum--);
        int index = flippedIndex.getOrDefault(i, i);
        flippedIndex.put(i, flippedIndex.getOrDefault(availableNum, availableNum));
        int[] pos = {index / cols, index % cols};
        return pos;
    }

    public void reset() {
        availableNum = rows * cols;
        flippedIndex.clear();
    }
}
```