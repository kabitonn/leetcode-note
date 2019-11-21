# 179. Largest Number(M)


[179. 最大数](https://leetcode-cn.com/problems/largest-number/)


## 题目描述(中等)

Given a list of non negative integers, arrange them such that they form the largest number.

Example 1:
```
Input: [10,2]
Output: "210"
```
Example 2:
```
Input: [3,30,34,5,9]
Output: "9534330"
```

**Note**: The result may be very large, so you need to return a string instead of an integer.


## 思路

- 自定义排序

## 解决方法



### 自定义排序规则

```
    public String largestNumber(int[] nums) {
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (compareTo(nums[i], nums[j]) < 0) {
                    swap(nums, i, j);
                }
            }
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < n; i++) {
            if (sb.length() == 0 && i != n - 1 && nums[i] == 0) {
                continue;
            }
            sb.append(nums[i]);
        }
        return sb.toString();
    }

    public int compareTo(int a, int b) {
        String ab = "" + a + b;
        String ba = "" + b + a;
        return ab.compareTo(ba);
    }

    public void swap(int[] nums, int i, int j) {
        nums[i] = nums[i] ^ nums[j];
        nums[j] = nums[i] ^ nums[j];
        nums[i] = nums[i] ^ nums[j];
    }

```


### 匿名内部类

```
    public String largestNumber1(int[] nums) {
        String[] strs = new String[nums.length];
        for (int i = 0; i < nums.length; i++) {
            strs[i] = String.valueOf(nums[i]);
        }
        Arrays.sort(strs, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                if (o1.length() == o2.length()) {
                    return o2.compareTo(o1);
                }
                String o12 = o1 + o2;
                String o21 = o2 + o1;
                return o21.compareTo(o12);
            }
        });
        if (strs[0].equals("0")) {
            return "0";
        }
        StringBuilder sb = new StringBuilder();
        for (String n : strs) {
            sb.append(n);
        }
        return sb.toString();
    }
```

采用函数表达式后速度慢了很多

```java
public String largestNumber2(int[] nums) {
        String[] strs = new String[nums.length];
        for (int i = 0; i < nums.length; i++) {
            strs[i] = String.valueOf(nums[i]);
        }
        Arrays.sort(strs, (a, b) -> {
            if (a.length() == b.length()) {
                return b.compareTo(a);
            }
            String ab = a + b;
            String ba = b + a;
            return ba.compareTo(ab);
        });
        if (strs[0].equals("0")) {
            return "0";
        }
        StringBuilder sb = new StringBuilder();
        for (String n : strs) {
            sb.append(n);
        }
        return sb.toString();
    }
```
