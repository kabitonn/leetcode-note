# 215. Kth Largest Element in an Array(M)

[215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

## 题目描述(中等)

Find the **kth** largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

Example 1:
```
Input: [3,2,1,5,6,4] and k = 2
Output: 5
```
Example 2:
```
Input: [3,2,3,1,2,4,5,5,6] and k = 4
Output: 4
```
**Note**:
You may assume k is always valid, 1 ≤ k ≤ array's length.

## 思路

- 完全排序
- 改进快排
- 堆排


## 解决方法

### 完全排序

```java
        public int findKthLargest0(int[] nums, int k) {
        Arrays.sort(nums);
        return nums[nums.length - k];
    }

```

### 改进快排

快排中每趟quickpass(partition)返回的下标即为所有数组中该元素最后应该在的位置，判断该下标和目标确定搜索区间

借助 partition 操作定位到最终排定以后索引为 len - k 的那个元素

partition（切分）操作，使得：

    - 对于某个索引 j，nums[j] 已经排定，即 nums[j] 经过 partition（切分）操作以后会放置在它 “最终应该放置的地方”；
    - nums[left] 到 nums[j - 1] 中的所有元素都不大于 nums[j]；
    - nums[j + 1] 到 nums[right] 中的所有元素都不小于 nums[j]。


普通快排，默认选择区间left为pivot元素，发现速度不太理想，下文修改pivot的选取
```java
    public int findKthLargest(int[] nums, int k) {
        int target = nums.length - k;
        int left = 0, right = nums.length - 1;
        while (left < right) {
            int pivot = quickPass(nums, left, right);
            if (pivot == target) {
                break;
            } else if (pivot > target) {
                right = pivot - 1;
            } else if (pivot < target) {
                left = pivot + 1;
            }
        }
        return nums[target];
    }

    public int quickPass(int[] nums, int left, int right) {
        int temp = nums[left];
        while (left < right) {
            while (left < right && nums[right] >= temp) {
                right--;
            }
            nums[left] = nums[right];
            while (left < right && nums[left] <= temp) {
                left++;
            }
            nums[right] = nums[left];
        }
        nums[left] = temp;
        return left;
    }

```

修改快排pivot 的选取，选取中位数
```java
    public int findKthLargest1(int[] nums, int k) {
        return quickSelect(nums, 0, nums.length - 1, nums.length - k);
    }

    public int quickSelect(int[] nums, int left, int right, int k) {
        if (left == right) {
            return nums[left];
        }
        int mid = left + (right - left) / 2;
        swap(nums, left, mid);
        int temp = nums[left];
        int start = left, end = right;
        while (left < right) {
            while (left < right && nums[right] >= temp) {
                right--;
            }
            nums[left] = nums[right];
            while (left < right && nums[left] <= temp) {
                left++;
            }
            nums[right] = nums[left];
        }
        nums[left] = temp;
        if (left < k) {
            return quickSelect(nums, left + 1, end, k);
        } else if (left > k) {
            return quickSelect(nums, start, left - 1, k);
        } else {
            return nums[left];
        }
    }

    public void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }

```
### 优先队列 堆排序

#### 大根堆

把 len 个元素都放入一个最大堆中，然后再 pop() 出 k - 1 个元素，因为前 k - 1 大的元素都被弹出了，此时最大堆的堆顶元素就是数组中的第 k 个最大元素。


```java
    public int findKthLargest2(int[] nums, int k) {
        int len = nums.length;
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>(len, (a, b) -> (b - a));
        for (int n : nums) {
            maxHeap.add(n);
        }
        for (int i = 0; i < k - 1; i++) {
            maxHeap.poll();
        }
        return maxHeap.peek();
    }

```

#### 小根堆

把 len 个元素都放入一个最小堆中，然后再 pop() 出 len - k 个元素，此时最小堆只剩下 k 个元素，堆顶元素就是数组中的第 k 个最大元素。

```java
    public int findKthLargest2_1(int[] nums, int k) {
        int len = nums.length;
        PriorityQueue<Integer> minHeap = new PriorityQueue<>(len);
        for (int n : nums) {
            minHeap.add(n);
        }
        for (int i = 0; i < len - k; i++) {
            minHeap.poll();
        }
        return minHeap.peek();
    }
```


只用 k 个容量的优先队列，而不用全部 len 个容量。

```java
    public int findKthLargest2_2(int[] nums, int k) {
        int len = nums.length;
        PriorityQueue<Integer> minHeap = new PriorityQueue<>(k);
        for (int i = 0; i < k; i++) {
            minHeap.add(nums[i]);
        }
        for (int i = k; i < len; i++) {
            if (nums[i] > minHeap.peek()) {
                minHeap.poll();
                minHeap.add(nums[i]);
            }
        }
        return minHeap.peek();
    }

```
用 k + 1 个容量的优先队列，使得上面的过程更“连贯”一些，到了 k 个以后的元素，就进来一个，出去一个，让优先队列自己去维护大小关系。

```java
    public int findKthLargest2_3(int[] nums, int k) {
        int len = nums.length;
        PriorityQueue<Integer> minHeap = new PriorityQueue<>(k + 1);
        for (int i = 0; i < k; i++) {
            minHeap.add(nums[i]);
        }
        for (int i = k; i < len; i++) {
            minHeap.add(nums[i]);
            minHeap.poll();
        }
        return minHeap.peek();
    }

```

### 数组 堆排序

#### 大根堆

大根堆堆顶取出k-1次，堆顶就是数组中的第 k 个最大元素。




```java
    public int findKthLargest3(int[] nums, int k) {
        buildMaxHeap(nums);
        int len = nums.length;
        int sorted = len;
        //剩余未排序数量
        for (int i = 0; i < k - 1; i++) {
            swap(nums, 0, --sorted);
            adjustMax(nums, 0, sorted);
        }
        return nums[0];
    }

    public void buildMaxHeap(int[] arr) {
        int len = arr.length;
        for (int i = len / 2; i >= 0; i--) {
            adjustMax(arr, i, len);
        }
    }

    public void adjustMax(int[] arr, int node, int size) {
        int left = 2 * node + 1;
        int right = 2 * node + 2;
        while (left < size) {
            int large = right < size && arr[right] > arr[left] ? right : left;
            if (arr[node] >= arr[large]) {
                break;
            }
            swap(arr, node, large);
            node = large;
            left = 2 * node + 1;
            right = 2 * node + 2;
        }
    }

```


#### 小根堆

小根堆堆顶取出len-k次，堆顶就是数组中的第 k 个最大元素。


```java
    public int findKthLargest4(int[] nums, int k) {
        buildMinHeap(nums);
        int len = nums.length;
        int sorted = len;
        //剩余未排序数量
        for (int i = k; i < len; i++) {
            swap(nums, 0, --sorted);
            adjustMin(nums, 0, sorted);
        }
        return nums[0];
    }

    public void buildMinHeap(int[] arr) {
        int len = arr.length;
        for (int i = len / 2; i >= 0; i--) {
            adjustMin(arr, i, len);
        }
    }

    public void adjustMin(int[] arr, int node, int size) {
        int left = 2 * node + 1;
        int right = 2 * node + 2;
        while (left < size) {
            int small = right < size && arr[right] < arr[left] ? right : left;
            if (arr[node] <= arr[small]) {
                break;
            }
            swap(arr, node, small);
            node = small;
            left = 2 * node + 1;
            right = 2 * node + 2;
        }
    }
```