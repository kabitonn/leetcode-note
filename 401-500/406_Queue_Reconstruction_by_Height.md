
# 406. Queue Reconstruction by Height(M)

[406. 根据身高重建队列](https://leetcode-cn.com/problems/queue-reconstruction-by-height/)

## 题目描述(中等)

假设有打乱顺序的一群人站成一个队列。 每个人由一个整数对`(h, k)`表示，其中h是这个人的身高，k是排在这个人前面且身高大于或等于h的人数。 编写一个算法来重建这个队列。

注意：
总人数少于1100人。

示例
```
输入:
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

输出:
[[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]
```


## 思路

- 排序 + 插入
- 排序 + 放置

## 解决方法

### 排序 + 插入

身高降序，k升序排列数组

K值定义为 排在h前面且身高大于或等于h的人数  
因为从身高降序开始插入，此时所有人身高都大于等于h  
因此K值即为需要插入的位置


```java
    public int[][] reconstructQueue(int[][] people) {
        Arrays.sort(people, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                //h降序 k升序
                return o1[0] == o2[0] ? o1[1] - o2[1] : o2[0] - o1[0];
            }
        });

        List<int[]> list = new LinkedList<>();
        for (int[] p : people) {
            list.add(p[1], p);
        }
        return list.toArray(new int[0][]);
    }

```

### 排序 + 放置

身高升序 k降序排列数组

- 取出身高最低元素，在**剩余可用索引中**选取其对应的k，剩余元素都是身高大于等于其的自身

```java
    public int[][] reconstructQueue1(int[][] people) {
        Arrays.sort(people, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                //h升序 k降序
                return o1[0] == o2[0] ? o2[1] - o1[1] : o1[0] - o2[0];
            }
        });
        List<Integer> unusedIndex = new LinkedList<>();
        for (int i = 0; i < people.length; i++) {
            unusedIndex.add(i);
        }
        int[][] queue = new int[people.length][2];
        for (int[] p : people) {
            Integer index = unusedIndex.get(p[1]);
            queue[index] = p;
            unusedIndex.remove(p[1]);
        }
        return queue;
    }
```