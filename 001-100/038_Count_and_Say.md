# 038. Count and Say(E)

[038. Count and Say](https://leetcode-cn.com/problems/count-and-say/)



## 题目描述(简单)

The count-and-say sequence is the sequence of integers with the first five terms as following:



```
1.     1
2.     11
3.     21
4.     1211
5.     111221

1 is read off as "one 1" or 11.
11 is read off as "two 1s" or 21.
21 is read off as "one 2, then one 1" or 1211.

```


Given an integer n where 1 ≤ n ≤ 30, generate the $n^{th}$ term of the count-and-say sequence.

**Note**: Each term of the sequence of integers will be represented as a string.

 

Example 1:
```
Input: 1
Output: "1"
```

Example 2:
```
Input: 4
Output: "1211"
```


## 思路

初始值第一行是 1。

第二行读第一行，1 个 1，去掉个字，所以第二行就是 11。

第三行读第二行，2 个 1，去掉个字，所以第三行就是 21。

第四行读第三行，1 个 2，1 个 1，去掉所有个字，所以第四行就是 1211。

第五行读第四行，1 个 1，1 个 2，2 个 1，去掉所有个字，所以第五航就是 111221。

第六行读第五行，3 个 1，2 个 2，1 个 1，去掉所以个字，所以第六行就是 312211。

然后题目要求输入 1 - 30 的任意行数，输出该行是什么。


## 解决方法

### 递归

```java
    public String countAndSay(int n) {
        if (n == 1) {
            return "1";
        }
        String last = countAndSay(n - 1);
        return getNextString(last);
    }
    private String getNextString1(String last){
        StringBuilder sb = new StringBuilder();
        int count = 1;
        for(int i=0;i<last.length();i++){
            if(i+1<last.length()&&last.charAt(i)==last.charAt(i+1)){
                count++;
            }
            else {
                sb.append(count).append(last.charAt(i));
                count=1;
            }
        }
        return sb.toString();
    }

    public String getNextString(String last) {
        if (last.length() == 0) {
            return "";
        }
        int repeatNum = getRepeatNum(last);
        return "" + repeatNum + last.charAt(0) + getNextString(last.substring(repeatNum));
    }
    
    private int getRepeatNum(String string) {
        int count = 1;
        char ch = string.charAt(0);
        for (int i = 1; i < string.length(); i++) {
            if (ch == string.charAt(i)) {
                count++;
            } else {
                break;
            }
        }
        return count;
    }

```


时间复杂度：。

空间复杂度：O(1)。

### 迭代
```java
    public String countAndSay1(int n){
        String res = "1";
        while(n-->1){
            String tmp = "";
            for(int i=0;i<res.length();i++){
                int repeatNum= getRepeatNum(res.substring(i));
                tmp = tmp+repeatNum+res.charAt(i);
                i=i+repeatNum-1;
            }
            res = tmp;
        }
        return res;
    }

    private int getRepeatNum(String string) {
        int count = 1;
        char ch = string.charAt(0);
        for (int i = 1; i < string.length(); i++) {
            if (ch == string.charAt(i)) {
                count++;
            } else {
                break;
            }
        }
        return count;
    }
```


```java
    public String countAndSay2(int n){
        String last = "1";
        while(n-->1){
            StringBuilder sb = new StringBuilder();
            int count = 1;
            for(int i=0;i<last.length();i++){
                if(i+1<last.length()&&last.charAt(i)==last.charAt(i+1)){
                    count++;
                }
                else {
                    sb.append(count).append(last.charAt(i));
                    count=1;
                }
            }
            last = sb.toString();
        }
        return last;
    }
```
