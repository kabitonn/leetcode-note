
# 551. Student Attendance Record I(E)
 
[551. 学生出勤记录 I](https://leetcode-cn.com/problems/student-attendance-record-i/)

## 题目描述(简单)

给定一个字符串来代表一个学生的出勤记录，这个记录仅包含以下三个字符：

- 'A' : Absent，缺勤
- 'L' : Late，迟到
- 'P' : Present，到场

如果一个学生的出勤记录中不超过一个'A'(缺勤)并且不超过两个连续的'L'(迟到),那么这个学生会被奖赏。

你需要根据这个学生的出勤记录判断他是否会被奖赏。

示例 1:
```
输入: "PPALLP"
输出: True
```

示例 2:
```
输入: "PPALLL"
输出: False
```


## 思路

## 解决方法

### 遍历统计

```java
    public boolean checkRecord(String s) {
        int a = 0;
        int l = 0;
        for (char c : s.toCharArray()) {
            if (c == 'A') {
                if (++a > 1) {
                    return false;
                }
                l = 0;
            } else if (c == 'L') {
                if (++l > 2) {
                    return false;
                }
            } else {
                l = 0;
            }
        }
        return true;
    }
```