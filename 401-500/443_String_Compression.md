# 443. String Compression(E)
[443. String Compression](https://leetcode-cn.com/problems/string-compression/)

## 题目描述(简单)

Given an array of characters, compress it **in-place**.

The length after compression must always be smaller than or equal to the original array.

Every element of the array should be a **character** (not int) of length 1.

After you are done modifying the input array in-place, return the new length of the array.

** Follow up:**
Could you solve it using only $O(1)$ extra space?

 
Example 1:
```
Input:
["a","a","b","b","c","c","c"]

Output:
Return 6, and the first 6 characters of the input array should be: ["a","2","b","2","c","3"]

Explanation:
"aa" is replaced by "a2". "bb" is replaced by "b2". "ccc" is replaced by "c3".
```

Example 2:
```
Input:
["a"]

Output:
Return 1, and the first 1 characters of the input array should be: ["a"]

Explanation:
Nothing is replaced.
```

Example 3:
```
Input:
["a","b","b","b","b","b","b","b","b","b","b","b","b"]

Output:
Return 4, and the first 4 characters of the input array should be: ["a","b","1","2"].

Explanation:
Since the character "a" does not repeat, it is not compressed. "bbbbbbbbbbbb" is replaced by "b12".
Notice each digit has it's own entry in the array.
```

**Note:**

1. All characters have an ASCII value in [35, 126].
2. 1 <= len(chars) <= 1000.


## 思路

## 解决方法

### 双指针
每次遇到新字符先写入旧字符

```java
	public int compress0(char[] chars) {
		int len=chars.length;
		int count=1;
		int index = 0;
		int i=0;
		for(;i<len;i++) {
			if(i+1<len&&chars[i]==chars[i+1]) {count++;}
			else if(i+1<len&&chars[i]!=chars[i+1]) {
				chars[index++] = chars[i];
				if(count!=1) {
					String strCount = Integer.toString(count);
					for(int j=0;j<strCount.length();j++) {
						chars[index++] = strCount.charAt(j);
					}
				}
				count = 1;
			}
		}
		chars[index++] = chars[len-1];
		if(count!=1) {
			String strCount = Integer.toString(count);
			for(int j=0;j<strCount.length();j++) {
				chars[index++] = strCount.charAt(j);
			}
		}
		return index;
	}
```



### 双指针
原地算法 双指针
每次遇到新字符直接写入新字符
index 已压缩的结果的末尾,
指针i指示未压缩字符串的开头
遇到相同的字符,指针i便向后滑动,直到遇到不同字符.指针i滑动的距离即为相同字符序列的长度

```java
    public int compress(char[] chars) {
    	int len=chars.length;
        int index = 0;
        int i=0;
        while(i<len) {
        	chars[index++] = chars[i];
        	int j=i+1;
        	while(j<len&&chars[j]==chars[i]) {j++;}
        	if(j-i!=1) {
        		String strCount = String.valueOf(j-1-1);
        		for(char c:strCount.toCharArray()) {chars[index++]=c;}
        		i=j;
        	}
        	i++;
        }
        return index;
    }
```


