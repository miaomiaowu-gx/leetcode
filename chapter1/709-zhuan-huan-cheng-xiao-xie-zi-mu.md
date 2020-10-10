## 709. 转换成小写字母 

```
实现函数 ToLowerCase()，该函数接收一个字符串参数 str，并将该字符串中的大写字母转换成小写字母，之后返回新的字符串。 

示例 1： 
输入: "Hello"
输出: "hello" 

示例 2： 
输入: "here"
输出: "here" 

示例 3： 
输入: "LOVELY"
输出: "lovely"

Related Topics 字符串 
```

### 方法一、ASCCII码

通过 ASCII 码表操作字符串即可。
a-z：97-122  
A-Z：65-90  
0-9：48-57

```java
class Solution {
    public String toLowerCase(String str) {
        if (str == null || str.length() == 0) {
            return str;
        }
        StringBuilder sb = new StringBuilder();
        for (char ch : str.toCharArray()) {
            // a-z：97-122  A-Z：65-90  0-9：48-57
            if (ch >= 'A' && ch <= 'Z') {
                sb.append((char)(ch + 32));
            } else {
                sb.append(ch);
            }
        }
        return sb.toString();
    }
}
```

```java
class Solution {
    public String toLowerCase(String str) {
        if (str == null || str.length() == 0) {
            return str;
        }
        char[] ch = str.toCharArray();
        for (int i = 0; i < str.length(); i++) {
            if (ch[i] >= 'A' && ch[i] <= 'Z') {
                ch[i] += 32;
            }
        }
        return String.valueOf(ch);
    }
}
```

### 方法二、位运算

用位运算的技巧：
* 大写变小写、小写变大写：字符 ^= 32;
* 大写变小写、小写变小写：字符 |= 32;
* 大写变大写、小写变大写：字符 &= 33;
SCII 码表中大写的 A 是 65，小写的 a 是 97，它们的差是 32
65 | 32 转为二进制（按8位来算）可以得到 0100 0001 | 0010 0000 = 0110 0001 = 97 = a。

```java
class Solution {
    public String toLowerCase(String str) {
        if (str == null || str.length() == 0) {
            return str;
        }
        char[] ch = str.toCharArray();
        for (int i = 0; i < str.length(); i++) {
            ch[i] |= 32;
        }
        return String.valueOf(ch);
    }
}
```





