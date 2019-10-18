# 066. Plus One(E)
[066. Plus One](https://leetcode-cn.com/problems/plus-one/)

## 题目描述(简单)

Given a non-empty array of digits representing a non-negative integer, plus one to the integer.

The digits are stored such that the most significant digit is at the head of the list, and each element in the array contain a single digit.

You may assume the integer does not contain any leading zero, except the number 0 itself.

Example 1:
```
Input: [1,2,3]
Output: [1,2,4]
Explanation: The array represents the integer 123.
```
Example 2:
```
Input: [4,3,2,1]
Output: [4,3,2,2]
Explanation: The array represents the integer 4321.
```
## 思路

1. 将1看做一个数
2. 1即为1

## 解决方法

### 两数相加

每位数字和进位相加

```java
	public int[] plusOne(int[] digits) {
		int n = digits.length;
		List<Integer> list = new ArrayList<>();
		int carry = 1;
		int num = 0;
		for(int i=n-1;i>=0;i--) {
			num = digits[i]+carry;
			carry = num/10;
			num %= 10;
			list.add(0,num);
		}
		if(carry!=0) {
			list.add(0, carry);
		}
		n = list.size();
    	int[] plusNum = new int[n];
    	for(int i=0;i<n;i++) {
    		plusNum[i] = list.get(i);
    	}
		return plusNum;
	}
```

时间复杂度：O(n)


### 个位加1

若有进位继续相加，进位时，carry为1，低位为0

```java
	public int[] plusOne(int[] digits) {
		int len = digits.length;
        for(int i = len - 1; i >= 0; i--) {
            digits[i]++;
            digits[i] %= 10;
            if(digits[i]!=0)
                return digits;
        }
        digits = new int[len + 1];
        digits[0] = 1;
        return digits;
	}
```

时间复杂度：O(n)

### 转换为数字相加


```java
    public int[] plusOne(int[] digits) {
        long num = 0;
    	for(int n:digits) {
        	num = num*10+n;
        }
    	num++;
    	List<Integer> list = new ArrayList<>();
    	while(num!=0) {
    		list.add((int)(num%10));
    		num/=10;
    	}
    	int n = list.size();
    	int[] plusNum = new int[n];
    	for(int i=0;i<n;i++) {
    		plusNum[i] = list.get(n-i-1);
    	}
    	return plusNum;
    }
```




