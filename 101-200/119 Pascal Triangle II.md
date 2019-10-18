## [119. Pascal's Triangle II](https://leetcode-cn.com/problems/pascals-triangle-ii/)

## 1. 题目描述\(简单\)

Given a non-negative index k where k ≤ 33, return the kth index row of the Pascal's triangle.

**Note **that the row index starts from 0.

![](/assets/101-200/119-problem-1.png)

In Pascal's triangle, each number is the sum of the two numbers directly above it.

Example:

```
Input: 3
Output: [1,3,3,1]
```

Follow up:

> Could you optimize your algorithm to use only O\(k\) extra space?

## 2. 思路

1. 每行正向遍历，根据上一行计算该行数据
2. 每行逆向遍历，当前行即可作为上一行

## 3. 解决方法

### 3.1 行内正向遍历


```java
    public List<Integer> getRow(int rowIndex) {
    	if(rowIndex==0) {return new ArrayList<>(Arrays.asList(1));}
    	//if(rowIndex==1) {return new ArrayList<>(Arrays.asList(1,1));}
    	List<Integer> row = null;
        List<Integer> preLine = new ArrayList<>(Arrays.asList(1));
        for(int i=1;i<=rowIndex;i++) {
        	row = new ArrayList<>();
        	row.add(1);
        	for(int j=1;j<i;j++) {
        		row.add(preLine.get(j-1)+preLine.get(j));
        	}
        	row.add(1);
        	preLine = row;
        }
        return row;
    }
```
时间复杂度 $$O(n^2)$$
空间复杂度 O(n)

### 3.2 行内逆向遍历

每行首尾为1，自后向前计算该位置数值，覆盖已不再使用的位置的数据

```java
    public List<Integer> getRow(int rowIndex) {
    	if(rowIndex==0) {return new ArrayList<>(Arrays.asList(1));}
    	Integer[] line = new Integer[rowIndex+1];
        for(int i=1;i<=rowIndex;i++) {
        	line[0] = 1;
        	line[i] = 1;
        	for(int j=i-1;j>=1;j--) {
        		line[j] = line[j]+line[j-1];
        	}
        }
        List<Integer> row = new ArrayList<>(Arrays.asList(line));
        return row;
    }
```
时间复杂度 $$O(n^2)$$
空间复杂度 O(n)





