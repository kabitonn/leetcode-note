
# 535. Encode and Decode TinyURL(M)

[535. TinyURL 的加密与解密](https://leetcode-cn.com/problems/encode-and-decode-tinyurl/)

## 题目描述(中等)

TinyURL是一种URL简化服务， 比如：当你输入一个URL https://leetcode.com/problems/design-tinyurl 时，它将返回一个简化的URL http://tinyurl.com/4e9iAk.

要求：设计一个 TinyURL 的加密 encode 和解密 decode 的方法。你的加密和解密算法如何设计和运作是没有限制的，你只需要保证一个URL可以被加密成一个TinyURL，并且这个TinyURL可以用解密方法恢复成原本的URL。


## 思路

## 解决方法

### 秘钥key

```java
final int priKey = 77;
    final int pubKey = 66;

    // Encodes a URL to a shortened URL.
    public String encode(String longUrl) {
        char[] chars = longUrl.toCharArray();
        int key = priKey ^ pubKey;
        for (int i = 0; i < chars.length; i++) {
            chars[i] ^= key;
        }
        return "http://" + new String(chars);
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        char[] chars = shortUrl.substring(7).toCharArray();
        int key = priKey ^ pubKey;
        for (int i = 0; i < chars.length; i++) {
            chars[i] ^= key;
        }
        return new String(chars);
    }
```