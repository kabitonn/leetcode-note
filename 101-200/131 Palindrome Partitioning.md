# 131. Palindrome Partitioning(M)


[131. 分割回文串](https://leetcode-cn.com/problems/palindrome-partitioning/)


## 题目描述(中等)

Given a string s, partition s such that every substring of the partition is a palindrome.

Return all possible palindrome partitioning of s.

Example:
```
Input: "aab"
Output:
[
  ["aa","b"],
  ["a","a","b"]
]
```


## 思路

- 分治

## 解决方法



### 分治

```java
    public List<List<String>> partition(String s) {
        return conquer(s);
    }

    public List<List<String>> conquer(String s) {
        List<List<String>> listList = new ArrayList<>();
        if (s.length() == 0) {
            listList.add(new ArrayList<>());
            return listList;
        }
        if (s.length() == 1) {
            listList.add(new ArrayList<>(Arrays.asList(s)));
            return listList;
        }
        for (int i = 0; i < s.length(); i++) {
            String leftStr = s.substring(0, i + 1);
            if (!isPalindrome(leftStr)) {
                continue;
            }
            String rightStr = s.substring(i + 1);
            for (List<String> list : conquer(rightStr)) {
                list.add(0, leftStr);
                listList.add(list);
            }
        }
        return listList;
    }

    private boolean isPalindrome(String s) {
        int i = 0;
        int j = s.length() - 1;
        while (i < j) {
            if (s.charAt(i) != s.charAt(j)) {
                return false;
            }
            i++;
            j--;
        }
        return true;
    }

```




