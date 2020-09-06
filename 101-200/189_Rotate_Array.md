# 189. Rotate Array(E)
[189. Rotate Array](https://leetcode-cn.com/problems/rotate-array/)

## 题目描述\(简单\)

Given an array, rotate the array to the right by k steps, where k is non-negative.

Example 1:

```
Input: [1,2,3,4,5,6,7] and k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]
```

Example 2:

```
Input: [-1,-100,3,99] and k = 2
Output: [3,99,-1,-100]
Explanation: 
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]
```

**Note**:

* Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.
* Could you do it in-place with O\(1\) extra space?

## 思路

## 解决方法

### 旋转k次

```java
    public void rotate(int[] nums, int k) {
        int len = nums.length;
        k%=len;
        if(len == 0||k==0) {return;}
        for(int i=0;i<k;i++) {
            int tmp = nums[len-1];
            for(int j=len-1;j>0;j--) {
                nums[j] = nums[j-1];
            }
            nums[0] = tmp;
        }
    }
```

时间复杂度：O(n*k)。每个元素都被移动 1 步O(n) k次O(k)） 。  
空间复杂度：O(1) 。没有额外空间被使用。

### 三次反转

```java
   public void rotate(int[] nums, int k) {
        int len = nums.length;
        k%=len;
        if(len == 0||k==0) {return;}
        reverse(nums, 0, len-1);
        reverse(nums, 0, k-1);
        reverse(nums, k, len-1);
    }
    public void reverse(int[] nums, int i,int j) {
        while(i<j) {
            int tmp = nums[i];
            nums[i] = nums[j]; 
            nums[j] = tmp;
            i++;
            j--;
        }
    }
```

时间复杂度：O(n)。 n 个元素被反转了总共 3 次。
空间复杂度：O(1)。 没有使用额外的空间。


### 环状替换

如果我们直接把每一个数字放到它最后的位置，但这样的后果是遗失原来的元素。因此，我们需要把被替换的数字保存在变量 temptemp 里面。然后，我们将被替换数字（temp）放到它正确的位置，并继续这个过程 n 次， n 是数组的长度。这是因为我们需要将数组里所有的元素都移动。但是，这种方法可能会有个问题，如果 $n\%k==0 0$，其中 $k=k\%n$ （因为如果 k 大于 n ，移动 k 次实际上相当于移动 $k\%n$次）。这种情况下，我们会发现在没有遍历所有数字的情况下回到出发数字。此时，我们应该从下一个数字开始再重复相同的过程。

现在，我们看看上面方法的证明。假设，数组里我们有 n 个元素并且 k 是要求移动的次数。更进一步，假设 $n\%k=0$ 。第一轮中，所有移动数字的下标 i 满足 $i\%k==0$。这是因为我们每跳 k 步，我们只会到达相距为 k 个位置下标的数。每一轮，我们都会移动 $\frac{n}{k}$个元素。下一轮中，我们会移动满足 $i\%k==1$ 的位置的数。这样的轮次会一直持续到我们再次遇到 $i\%k==0$ 的地方为止，此时 i=k 。此时在正确位置上的数字共有$k \times \frac{n}{k}=n$个。因此所有数字都在正确位置上。


```
nums: [1, 2, 3, 4, 5, 6]
k: 2
```

![](../assets/leetcode-note/101-200/189-s-3-1.png)

```java
    public void rotate(int[] nums,int k) {
        int len = nums.length;
        k%=len;
        if(len == 0||k==0) {return;}
        int count = 0;
        for (int start = 0; count < nums.length; start++) {
            int current = start;
            int prev = nums[start];
            do {
                current = (current + k) % nums.length;
                int temp = nums[current];
                nums[current] = prev;
                prev = temp;
                count++;
            } while (start != current);
        }
    }
```

时间复杂度：O(n)。只遍历了每个元素一次。  
空间复杂度：O(1) 。使用了常数个额外空间。

