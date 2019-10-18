# 412. Fizz Buzz
[412. Fizz Buzz](https://leetcode-cn.com/problems/fizz-buzz/)

## 题目描述(简单)

Write a program that outputs the string representation of numbers from 1 to n.

But for multiples of three it should output “Fizz” instead of the number and for the multiples of five output “Buzz”. For numbers which are multiples of both three and five output “FizzBuzz”.

Example:
```
n = 15,

Return:
[
    "1",
    "2",
    "Fizz",
    "4",
    "Buzz",
    "Fizz",
    "7",
    "8",
    "Fizz",
    "Buzz",
    "11",
    "Fizz",
    "13",
    "14",
    "FizzBuzz"
]
```

## 思路

## 解决方法

### 字符串连接

1. 能不能被 3 整除
2. 能不能被 5 整除
3. 不能被 3，5其中任何一个数整除

```java
    public List<String> fizzBuzz(int n) {
    	List<String> list = new ArrayList<>();
    	for(int i=1;i<=n;i++) {
    		String str = "";
    		if(i%3==0) {str += "Fizz";}
    		if(i%5==0) {str += "Buzz";}
    		if(str.equals("")) {str += i;}
    		list.add(str);
    	}
    	return list;
    }
```



### 模拟法

1. 初始化一个空的答案列表。
2. 遍历 $$1 ... N$$
3. 对于每个数，判断它能不能同时被 3 和 5 整除，如果可以就把 FizzBuzz 加入答案列表。
4. 如果不行，判断它能不能被 3 整除，如果可以，把 Fizz 加入答案列表。
5. 如果还是不行，判断它能不能被 5 整除，如果可以，把 Buzz 加入答案列表。
6. 如果以上都不行，把这个数加入答案列表。


```java
	public List<String> fizzBuzz(int n) {
		List<String> list = new ArrayList<>();
		for(int i=1;i<=n;i++) {
			String str = null;
			if(i%3==0&&i%5==0){str = "FizzBuzz";}
			else if(i%3==0){str = "Fizz";}
			else if(i%5==0){str = "Buzz";}
			else {str = String.valueOf(i);}
			list.add(str);
		}
		return list;
	}
```



