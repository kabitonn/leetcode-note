# 380. Insert Delete GetRandom O(1)(M)

[380. 常数时间插入、删除和获取随机元素](https://leetcode-cn.com/problems/insert-delete-getrandom-o1/)

## 题目描述(中等)

设计一个支持在平均 时间复杂度 O(1) 下，执行以下操作的数据结构。

1. `insert(val)`：当元素 val 不存在时，向集合中插入该项。
2. `remove(val)`：元素 val 存在时，从集合中移除该项。
3. `getRandom()`：随机返回现有集合中的一项。每个元素应该有相同的概率被返回。

示例 :
```
// 初始化一个空的集合。
RandomizedSet randomSet = new RandomizedSet();

// 向集合中插入 1 。返回 true 表示 1 被成功地插入。
randomSet.insert(1);

// 返回 false ，表示集合中不存在 2 。
randomSet.remove(2);

// 向集合中插入 2 。返回 true 。集合现在包含 [1,2] 。
randomSet.insert(2);

// getRandom 应随机返回 1 或 2 。
randomSet.getRandom();

// 从集合中移除 1 ，返回 true 。集合现在包含 [2] 。
randomSet.remove(1);

// 2 已在集合中，所以返回 false 。
randomSet.insert(2);

// 由于 2 是集合中唯一的数字，getRandom 总是返回 2 。
randomSet.getRandom();
```

```java

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet obj = new RandomizedSet();
 * boolean param_1 = obj.insert(val);
 * boolean param_2 = obj.remove(val);
 * int param_3 = obj.getRandom();
 */
```

## 思路

- 哈希集+List
- 哈希集
- 两个哈希表

## 解决方法

### 哈希集+List

Set：插入，移除
List：随机获取

移除不满足O(1)

```java
public class RandomizedSet {

    Set<Integer> set;
    List<Integer> list;

    /**
     * Initialize your data structure here.
     */
    public RandomizedSet() {
        set = new HashSet<>();
        list = new ArrayList<>();
    }

    /**
     * Inserts a value to the set. Returns true if the set did not already contain the specified element.
     */
    public boolean insert(int val) {
        boolean flag = set.add(val);
        if (flag) {
            list.add(val);
        }
        return flag;
    }

    /**
     * Removes a value from the set. Returns true if the set contained the specified element.
     */
    public boolean remove(int val) {
        boolean flag = set.remove(val);
        if (flag) {
            list.remove((Integer)val);
        }
        return flag;
    }

    /**
     * Get a random element from the set.
     */
    public int getRandom() {
        int random = (int) (Math.random() * list.size());
        return list.get(random);
    }
}
```

### 哈希集

随机获取时利用数组

```java
public class RandomizedSet_1 {

    Set<Integer> set;

    /**
     * Initialize your data structure here.
     */
    public RandomizedSet_1() {
        set = new HashSet<>();
    }

    /**
     * Inserts a value to the set. Returns true if the set did not already contain the specified element.
     */
    public boolean insert(int val) {
        return set.add(val);
    }

    /**
     * Removes a value from the set. Returns true if the set contained the specified element.
     */
    public boolean remove(int val) {
        return set.remove(val);
    }

    /**
     * Get a random element from the set.
     */
    public int getRandom() {
        Integer[] array = set.toArray(new Integer[0]);
        int random = (int) (Math.random() * set.size());
        return array[random];
    }
}
```

### 两个哈希表

两个HashMap 
- 一个存储<index,value>
- 一个存储<value,index>

```java
public class RandomizedSet_2 {

    Map<Integer, Integer> valueMap;
    Map<Integer, Integer> indexMap;
    int size;

    /**
     * Initialize your data structure here.
     */
    public RandomizedSet_2() {
        indexMap = new HashMap<>();
        valueMap = new HashMap<>();
        size = 0;
    }

    /**
     * Inserts a value to the set. Returns true if the set did not already contain the specified element.
     */
    public boolean insert(int val) {
        if (indexMap.containsKey(val)) {
            return false;
        }
        indexMap.put(val, ++size);
        valueMap.put(size, val);
        return true;
    }

    /**
     * Removes a value from the set. Returns true if the set contained the specified element.
     */
    public boolean remove(int val) {
        if (!indexMap.containsKey(val)) {
            return false;
        }
        int index = indexMap.remove(val);
        if (index != size) {
            valueMap.remove(index);
            int lastValue = valueMap.get(size);
            valueMap.put(index, lastValue);
            indexMap.put(lastValue, index);
        }
        valueMap.remove(size--);
        return true;
    }

    /**
     * Get a random element from the set.
     */
    public int getRandom() {
        int randomIndex = new Random().nextInt(size) + 1;
        return valueMap.get(randomIndex);
    }
}
```

### 两个哈希表 + 空白索引列表

- 删除元素时，索引加入空白索引列表
- 添加元素时，若空白索引列表不为空，在空白位置上添加

```java
public class RandomizedSet_3 {

    Map<Integer, Integer> indexValue;
    Map<Integer, Integer> valueIndex;
    List<Integer> absentList;
    int size;

    /**
     * Initialize your data structure here.
     */
    public RandomizedSet_3() {
        valueIndex = new HashMap<>();
        indexValue = new HashMap<>();
        absentList = new ArrayList<>();
        size = 0;
    }

    /**
     * Inserts a value to the set. Returns true if the set did not already contain the specified element.
     */
    public boolean insert(int val) {
        if (valueIndex.containsKey(val)) {
            return false;
        }
        Integer index = absentList.size() != 0 ? absentList.get(0) : null;
        if (index == null) {
            size++;
            index = size;
        } else {
            absentList.remove(0);
        }
        valueIndex.put(val, index);
        indexValue.put(index, val);
        return true;
    }

    /**
     * Removes a value from the set. Returns true if the set contained the specified element.
     */
    public boolean remove(int val) {
        if (!valueIndex.containsKey(val)) {
            return false;
        }
        int index = valueIndex.remove(val);
        indexValue.remove(index);
        absentList.add(index);
        return true;
    }

    /**
     * Get a random element from the set.
     */
    public int getRandom() {
        int randomIndex;
        while (true) {
            randomIndex = new Random().nextInt(size) + 1;
            if (indexValue.containsKey(randomIndex)) {
                break;
            }
        }
        return indexValue.get(randomIndex);
    }
}
```