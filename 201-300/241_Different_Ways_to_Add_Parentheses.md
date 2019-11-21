# 241. Different Ways to Add Parentheses(M)

[]()

## 题目描述(中等)

Given a string of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. The valid operators are +, - and *.

Example 1:
```
Input: "2-1-1"
Output: [0, 2]
Explanation: 
((2-1)-1) = 0 
(2-(1-1)) = 2
```
Example 2:
```
Input: "2*3-4*5"
Output: [-34, -14, -10, -10, 10]
Explanation: 
(2*(3-(4*5))) = -34 
((2*3)-(4*5)) = -14 
((2*(3-4))*5) = -10 
(2*((3-4)*5)) = -10 
(((2*3)-4)*5) = 10
```

## 思路

- 递归分治

## 解决方法

### 递归分治


字符串以运算符划分为左右两部分，计算表达式结果

```java
    public List<Integer> diffWaysToCompute(String input) {
        return partition(input);
    }

    public List<Integer> partition(String input) {
        List<Integer> list = new ArrayList<>();
        if (!input.contains("+") && !input.contains("-") && !input.contains("*")) {
            list.add(Integer.valueOf(input));
            return list;
        }
        for (int i = 0; i < input.length(); i++) {
            if (input.charAt(i) == '+' || input.charAt(i) == '-' || input.charAt(i) == '*') {
                for (int left : partition(input.substring(0, i))) {
                    for (int right : partition(input.substring(i + 1))) {
                        if (input.charAt(i) == '+') {
                            list.add(left + right);
                        } else if (input.charAt(i) == '-') {
                            list.add(left - right);
                        } else if (input.charAt(i) == '*') {
                            list.add(left * right);
                        }
                    }
                }
            }
        }
        return list;
    }

```

### 递归 记忆化

```java
    public List<Integer> diffWaysToCompute0(String input) {
        return partition0(input, new HashMap<String, List<Integer>>());
    }

    public List<Integer> partition0(String input, Map<String, List<Integer>> map) {
        if (map.containsKey(input)) {
            return map.get(input);
        }
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < input.length(); i++) {
            if (input.charAt(i) == '+' || input.charAt(i) == '-' || input.charAt(i) == '*') {
                for (int left : partition0(input.substring(0, i),map)) {
                    for (int right : partition0(input.substring(i + 1),map)) {
                        if (input.charAt(i) == '+') {
                            list.add(left + right);
                        } else if (input.charAt(i) == '-') {
                            list.add(left - right);
                        } else if (input.charAt(i) == '*') {
                            list.add(left * right);
                        }
                    }
                }
            }
        }
        if(list.size()==0){
            list.add(Integer.valueOf(input));
        }
        map.put(input, list);
        return list;
    }
```


### 递归分治2

根据运算符的下标将运算数划分为左右两部分，计算结果

```java
    public List<Integer> diffWaysToCompute1(String input) {
        String[] numStrs = input.split("[\\+\\-\\*]");
        List<Character> opList = new ArrayList<>();
        for (char c : input.toCharArray()) {
            if (c == '+' || c == '-' || c == '*') {
                opList.add(c);
            }
        }
        return partition(numStrs, opList, 0, numStrs.length - 1);
    }

    public List<Integer> partition(String[] nums, List<Character> opList, int start, int end) {
        List<Integer> list = new ArrayList<>();
        if (start == end) {
            list.add(Integer.valueOf(nums[start]));
            return list;
        }
        for (int i = start; i < end; i++) {
            for (int left : partition(nums, opList, start, i)) {
                for (int right : partition(nums, opList, i + 1, end)) {
                    char op = opList.get(i);
                    if (op == '+') {
                        list.add(left + right);
                    } else if (op == '-') {
                        list.add(left - right);
                    } else if (op == '*') {
                        list.add(left * right);
                    }
                }
            }
        }
        return list;
    }
```

记忆化

```java
    public List<Integer> diffWaysToCompute2(String input) {
        String[] numStrs = input.split("[\\+\\-\\*]");
        List<Character> opList = new ArrayList<>();
        for (char c : input.toCharArray()) {
            if (c == '+' || c == '-' || c == '*') {
                opList.add(c);
            }
        }
        int n = numStrs.length;
        List<Integer>[][] memo = new List[n][n];
        return partition2(numStrs, opList, 0, numStrs.length - 1, memo);
    }

    public List<Integer> partition2(String[] nums, List<Character> opList, int start, int end, List<Integer>[][] memo) {
        if (memo[start][end] != null) {
            return memo[start][end];
        }
        List<Integer> list = new ArrayList<>();
        if (start == end) {
            list.add(Integer.valueOf(nums[start]));
            memo[start][end] = list;
            return list;
        }
        for (int i = start; i < end; i++) {
            for (int left : partition2(nums, opList, start, i, memo)) {
                for (int right : partition2(nums, opList, i + 1, end, memo)) {
                    char op = opList.get(i);
                    if (op == '+') {
                        list.add(left + right);
                    } else if (op == '-') {
                        list.add(left - right);
                    } else if (op == '*') {
                        list.add(left * right);
                    }
                }
            }
        }
        memo[start][end] = list;
        return list;
    }
```