
# 506. Relative Ranks(E)

[506. 相对名次](https://leetcode-cn.com/problems/relative-ranks/)

## 题目描述(简单)

给出 **N** 名运动员的成绩，找出他们的相对名次并授予前三名对应的奖牌。前三名运动员将会被分别授予 “金牌”，“银牌” 和“ 铜牌”（"Gold Medal", "Silver Medal", "Bronze Medal"）。

(注：分数越高的选手，排名越靠前。)

示例 1:
```
输入: [5, 4, 3, 2, 1]
输出: ["Gold Medal", "Silver Medal", "Bronze Medal", "4", "5"]
解释: 前三名运动员的成绩为前三高的，因此将会分别被授予 “金牌”，“银牌”和“铜牌” ("Gold Medal", "Silver Medal" and "Bronze Medal").
余下的两名运动员，我们只需要通过他们的成绩计算将其相对名次即可。
```

**提示**:

- N 是一个正整数并且不会超过 10000。
- 所有运动员的成绩都不相同。

## 思路

创建每一位运动员成绩相对分数的排名(列表索引值)，按照原运动员位置,置放所排名次

## 解决方法

### 排序 索引Map

```java
    public String[] findRelativeRanks(int[] nums) {
        String[] rankString = {"Gold Medal", "Silver Medal", "Bronze Medal"};
        int n = nums.length;
        int[] scores = Arrays.copyOf(nums, n);
        Arrays.sort(scores);
        Map<Integer, String> rankMap = new HashMap<>(n);
        for (int i = n - 1; i >= 0; i--) {
            if (i >= n - 3) {
                rankMap.put(scores[i], rankString[n - i - 1]);
            } else {
                rankMap.put(scores[i], Integer.toString(n - i));
            }
        }
        String[] ranks = new String[n];
        for (int i = 0; i < n; i++) {
            ranks[i] = rankMap.get(nums[i]);
        }
        return ranks;
    }
```


### 桶计数索引 排序

```java
    public String[] findRelativeRanks1(int[] nums) {
        int max = 0;
        for (int n : nums) {
            max = Math.max(max, n);
        }
        Integer[] rankMap = new Integer[max + 1];
        for (int i = 0; i < nums.length; i++) {
            rankMap[nums[i]] = i;
        }
        String[] rankString = {"Gold Medal", "Silver Medal", "Bronze Medal"};
        String[] ranks = new String[nums.length];
        int rank = 1;
        for (int i = max; i >= 0; i--) {
            if (rankMap[i] == null) {
                continue;
            }
            if (rank <= 3) {
                ranks[rankMap[i]] = rankString[rank - 1];
            } else {
                ranks[rankMap[i]] = Integer.toString(rank);
            }
            rank++;
        }
        return ranks;
    }
```