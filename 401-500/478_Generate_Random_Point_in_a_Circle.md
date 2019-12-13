
# 478. Generate Random Point in a Circle(M)

[478. 在圆内随机生成点](https://leetcode-cn.com/problems/generate-random-point-in-a-circle/)

## 题目描述(中等)

给定圆的半径和圆心的 x、y 坐标，写一个在圆中产生均匀随机点的函数 randPoint 。

说明:
- 输入值和输出值都将是浮点数。
- 圆的半径和圆心的 x、y 坐标将作为参数传递给类的构造函数。
- 圆周上的点也认为是在圆中。
- randPoint 返回一个包含随机点的x坐标和y坐标的大小为2的数组。

示例 1：
```
输入: 
["Solution","randPoint","randPoint","randPoint"]
[[1,0,0],[],[],[]]
输出: [null,[-0.72939,-0.65505],[-0.78502,-0.28626],[-0.83119,-0.19803]]
```

示例 2：
```
输入: 
["Solution","randPoint","randPoint","randPoint"]
[[10,5,-7.5],[],[],[]]
输出: [null,[11.52438,-8.33273],[2.46992,-16.21705],[11.13430,-12.42337]]
```

**输入语法说明**：

输入是两个列表：调用成员函数名和调用的参数。`Solution` 的构造函数有三个参数，圆的半径、圆心的 x 坐标、圆心的 y 坐标。`randPoint` 没有参数。输入参数是一个列表，即使参数为空，也会输入一个 [] 空列表。


## 思路

- 在正方形范围内随机生成点直到点在园内
- 极坐标生成圆内一点

## 解决方法

### 方形拒绝采样

在正方形范围内随机生成点直到点在园内

```java
public class S478GenerateRandomPointInACircle {

    private double radius;
    private double xCenter;
    private double yCenter;

    public S478GenerateRandomPointInACircle(double radius, double x_center, double y_center) {
        this.radius = radius;
        this.xCenter = x_center;
        this.yCenter = y_center;
    }

    public double[] randPoint() {
        Random random = new Random();
        double xRand, yRand;
        double distance;
        do {
            xRand = xCenter - radius + random.nextDouble() * radius * 2;
            yRand = yCenter - radius + random.nextDouble() * radius * 2;
            distance = Math.pow(xRand - xCenter, 2) + Math.pow(yRand - yCenter, 2);

        } while (distance > radius * radius);
        return new double[]{xRand, yRand};
    }

    public double[] randPoint0() {
        Random random = new Random();
        double dx;
        double dy;
        do {
            dx = random.nextDouble() * 2 - 1;
            dy = random.nextDouble() * 2 - 1;
        } while (dx * dx + dy * dy > 1);
        double xRand = xCenter + radius * dx;
        double yRand = yCenter + radius * dy;
        return new double[]{xRand, yRand};
    }
}
```

随机生成距离圆心dx,dy的点，然后随机生成方向

```java
    public double[] randPoint1() {
        Random random = new Random();
        double dx = random.nextDouble();
        double dy = random.nextDouble();
        while (dx * dx + dy * dy > 1) {
            dx = random.nextDouble();
            dy = random.nextDouble();
        }
        double xRand = xCenter + (random.nextBoolean() ? 1 : -1) * radius * dx;
        double yRand = yCenter + (random.nextBoolean() ? 1 : -1) * radius * dy;
        return new double[]{xRand, yRand};
    }

```

### 随机生成极坐标计算点坐标

- 通过：`double r = Math.sqrt(Math.pow(radius, 2) * random.nextDouble());`
- 有误：`double r = (radius * random.nextDouble());`

```java
    public double[] randPoint2() {
        Random random = new Random();
        double theta = random.nextDouble() * Math.PI * 2;
        double r = Math.sqrt(Math.pow(radius, 2) * random.nextDouble());
        double xRand = xCenter + Math.cos(theta) * r;
        double yRand = yCenter + Math.sin(theta) * r;
        return new double[]{xRand, yRand};
    }

```