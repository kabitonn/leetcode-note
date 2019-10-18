## [125. Valid Palindrome](https://leetcode-cn.com/problems/valid-palindrome/)

## 1. 题目描述(简单)

Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

**Note**: For the purpose of this problem, we define empty string as valid palindrome.

Example 1:
```
Input: "A man, a plan, a canal: Panama"
Output: true
```
Example 2:
```
Input: "race a car"
Output: false
```


## 2. 思路

1. 首尾双指针，非字母数字字符跳过，忽略大小写判断是否相等
2. 字符串转换为统一大小写字母

## 3. 解决方法

### 3.1



```java
    public boolean isPalindrome(String s) {
        int i=0;
        int j=s.length()-1;
        char[] str = s.toCharArray();
        while(i<j) {
        	if(!isLetterOrDigit(str[i])) {
        		i++;
        		continue;
        	}
        	if(!isLetterOrDigit(str[j])) {
        		j--;
        		continue;
        	}
        	if(!equal(str[i], str[j])) {return false;}
        	else {
				i++;
				j--;
			}
        }
        return true;
    }
    public boolean isLetterOrDigit(char c) {
    	if(c>='a'&&c<='z') {return true;}
    	if(c>='A'&&c<='Z') {return true;}
    	if(c>='0'&&c<='9') {return true;}
    	return false;
    }
    public boolean equal(char c1,char c2) {
		if(c1==c2) {return true;}
		if(((c1>='a'&&c1<='z'||c1>='A'&&c1<='Z')&&(c2>='a'&&c2<='z'||c2>='A'&&c2<='Z'))&&(c1-c2=='A'-'a'||c1-c2=='a'-'A')) {
			return true;
		}
		return false;
	}
```



### 3.2


```java
	public boolean isPalindrome1(String s) {
        int i=0;
        int j=s.length()-1;
        char[] str = s.toUpperCase().toCharArray();
        while(i<j) {
			while (i < j && !Character.isLetterOrDigit(str[i])) i++;
			while (i < j && !Character.isLetterOrDigit(str[j])) j--;
        	if(str[i]!=str[j]) {return false;}
        	else {
				i++;
				j--;
			}
        }
        return true;
    }
```



