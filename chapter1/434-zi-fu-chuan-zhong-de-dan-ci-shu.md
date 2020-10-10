# 434 字符串中的单词数

```text
统计字符串中的单词个数，【这里的单词指的是连续的不是空格的字符】。 
请注意，你可以假定字符串里不包括任何不可打印的字符。 
示例: 
输入: "Hello, my name is John"
输出: 5
解释: 这里的单词是指连续的不是空格的字符，所以 "Hello," 算作 1 个单词。

Related Topics 字符串
```

## 方法一、原地法

算法：计算单词的数量，就等同于计数单词开始的下标个数。若该下标前为空格（或者为初始下标），且自身不为空格，则其为单词开始的下标。

```java
class Solution {
    public int countSegments(String s) {
        int res = 0;
        for(int i=0; i<s.length(); i++){
            if((i==0 || s.charAt(i-1)==' ')&& s.charAt(i)!=' '){
                res++;
            }
        }
        return res;
    }
}
```

## 方法二、正则方式 1

考虑边缘情况：

* 开头的一个或多个空格会导致 split 函数在字符串开头产生一个错误的 ""，因此使用内置的 trim 函数来移除这些空格。
* 如果结果为空字符串，可以直接输出 0。因为：

  ```java
  String[] tokens = "".split("\\s++");
  tokens.length; // 1
  tokens[0]; // ""
  ```

* 将修整过的字符串以一个或多个空格字符切分（split 函数可以使用正则表达式），并返回结果数组的长度。

```java
class Solution {
    public int countSegments(String s) {
        String trimmed = s.trim();
        if (trimmed.equals("")) {
            return 0;
        }
        return trimmed.split("\\s+").length;
    }
}
```

## 方法三、正则方式 2

```java
class Solution {
    public int countSegments(String s) {
        s = s.replaceAll("[^ ]+ *","x");
        return s.trim().length();
    }
}
```

