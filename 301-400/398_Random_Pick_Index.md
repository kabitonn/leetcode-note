# 398. Random Pick Index(M)

[398. 随机数索引](https://leetcode-cn.com/problems/random-pick-index/)

## 题目描述(中等)

给定一个可能含有重复元素的整数数组，要求随机输出给定的数字的索引。 您可以假设给定的数字一定存在于数组中。

注意：
数组大小可能非常大。 使用太多额外空间的解决方案将不会通过测试。

示例:
```
int[] nums = new int[] {1,2,3,3,3};
Solution solution = new Solution(nums);

// pick(3) 应该返回索引 2,3 或者 4。每个索引的返回概率应该相等。
solution.pick(3);

// pick(1) 应该返回 0。因为只有nums[0]等于1。
solution.pick(1);
```
## 思路

- 哈希表+List
- 蓄水池抽样法

## 解决方法

### 哈希表+List

```java
public class S398RandomPickIndex {

    Map<Integer, List<Integer>> listMap;
    int[] nums;
    Random random;

    public S398RandomPickIndex(int[] nums) {
        random = new Random();
        this.nums = Arrays.copyOf(nums, nums.length);
        listMap = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if (!listMap.containsKey(nums[i])) {
                listMap.put(nums[i], new ArrayList<>());
            }
            listMap.get(nums[i]).add(i);
        }
    }

    public int pick(int target) {
        List<Integer> list = listMap.get(target);
        int index = random.nextInt(list.size());
        return list.get(index);
    }

    public static void main(String[] args) {
        int[] nums = {1, 2, 3, 3, 3};
        S398RandomPickIndex solution = new S398RandomPickIndex(nums);
        System.out.println(solution.pick(3));
    }
}

```
### 蓄水池抽样法

从左向右遍历数组，每遇到一个target，计数器 count 加1。然后生成随机数j在范围 [0, count-1] 中。如果j < 1 那么，取此位置。如果不是，则不取

第 count 个元素被选中的概率为 1 / count 。蓄水池中被选中要替换的元素的概率为1/1。综合看第 count 个元素在蓄水池中的概率为 1 / count * 1/1 = 1 / count 



```java
public class S398RandomPickIndex_1 {

    int[] nums;
    Random random;

    public S398RandomPickIndex_1(int[] nums) {
        random = new Random();
        this.nums = Arrays.copyOf(nums, nums.length);
    }

    public int pick(int target) {
        int index = -1;
        int count = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == target) {
                count++;
                if (random.nextInt(count) == 0) {
                    index = i;
                }
            }
        }
        return index;
    }

    public static void main(String[] args) {
        int[] nums = {1, 2, 3, 3, 3};
        S398RandomPickIndex_1 solution = new S398RandomPickIndex_1(nums);
        System.out.println(solution.pick(3));
    }
}

```