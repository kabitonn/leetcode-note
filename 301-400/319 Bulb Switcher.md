# 319. Bulb Switcher(M)

[319. 灯泡开关](https://leetcode-cn.com/problems/bulb-switcher/)

## 题目描述(中等)

初始时有 n 个灯泡关闭。 第 1 轮，你打开所有的灯泡。 第 2 轮，每两个灯泡你关闭一次。 第 3 轮，每三个灯泡切换一次开关（如果关闭则开启，如果开启则关闭）。第 i 轮，每 i 个灯泡切换一次开关。 对于第 n 轮，你只切换最后一个灯泡的开关。 找出 n 轮后有多少个亮着的灯泡。

示例:
```
输入: 3
输出: 1 
解释: 
初始时, 灯泡状态 [关闭, 关闭, 关闭].
第一轮后, 灯泡状态 [开启, 开启, 开启].
第二轮后, 灯泡状态 [开启, 关闭, 开启].
第三轮后, 灯泡状态 [开启, 关闭, 关闭]. 

你应该返回 1，因为只有一个灯泡还亮着。
```


## 思路

- 模拟过程
- 数学规律

## 解决方法

### 模拟过程

模拟题目描述的过程，每一次循环改变需要变换的开关状态

```java
    public int bulbSwitch(int n) {
        boolean[] switchs = new boolean[n];
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                if (j % i == 0) {
                    switchs[j - 1] = !switchs[j - 1];
                }
            }
        }
        int count = 0;
        for (int i = 0; i < n; i++) {
            count += switchs[i] ? 1 : 0;
        }
        return count;
    }
```

### 数学规律

每次将i的倍数的灯开关切换。我们可以发现，任何一个数总能拆解成两个数的乘积（假设编号为8的灯，会在第1、8次和2、4次被开关），从而导致开关状态不变。那么只有平方数的开关状态会被改变（自己和自己的乘积）


```java
    public int bulbSwitch1(int n) {
        return (int) Math.sqrt(n);
    }
```