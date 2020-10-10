## 520. 检测大写字母  

```
给定一个单词，你需要判断单词的大写使用是否正确。 

我们定义，在以下情况时，单词的大写用法是正确的： 
- 全部字母都是大写，比如"USA"。 
- 单词中所有字母都不是大写，比如"leetcode"。 
- 如果单词不只含有一个字母，只有首字母大写， 比如 "Google"。 
否则，我们定义这个单词没有正确使用大写字母。 

示例 1: 
输入: "USA"
输出: True

示例 2: 
输入: "FlaG"
输出: False

注意: 输入是由大写和小写拉丁字母组成的非空单词。 
Related Topics 字符串 
```


### 方式一、统计大写字母个数🍓

统计大写字母个数，然后分三种情况：
1. 大写字母个数为 0，或者大写字母个数为字符串长度，说明此时为全大写或者全小写，返回 true。
2. 首字母大写，大写字母个数为 1，说明此时只有首字母大写，返回true。

```java
class Solution {
    public boolean detectCapitalUse(String word) {
        int len = word.length();
        int count = 0;
        for(int i=0; i<len; i++){
            if(word.charAt(i)>='A' && word.charAt(i)<='Z')
                count++;
        }
        return count==len || count==0 || (count==1 && word.charAt(0)>='A'&& word.charAt(0)<='Z');

    }
}
```


### 方式二、由第一和第二字符来确定

【不推荐】

这里的约束条件其实通过第一和第二字符来确定。
第一个字符是小写，则整个单词都必须是小写。
第一个字符是大写，而第二个字符是小写，则后续所有字符也必须小写。
第一个字符是大写，而第二个字符是大写，则后续所有字符也必须大写。
否则上述规则，就是true，否则就是false。

```java
class Solution {
    public boolean detectCapitalUse(String word) {
        boolean ans = true;
        char firstChar = word.charAt(0);
        if ((firstChar >= 'A') && (firstChar <= 'Z')) {
            if (word.length() >= 2) {
                char secondChar = word.charAt(1);
                if ((secondChar >= 'A') && (secondChar <= 'Z')) {
                    for (int i = 2; i < word.length(); i++) {
                        char currChar = word.charAt(i);
                        if (!((currChar >= 'A') && (currChar <= 'Z'))) {
                            ans = false;
                            break;
                        };
                    };
                } else {
                    for (int i = 2; i < word.length(); i++) {
                        char currChar = word.charAt(i);
                        if ((currChar >= 'A') && (currChar <= 'Z')) {
                            ans = false;
                            break;
                        };
                    };
                }
            };
        } else {
            for (int i = 1; i < word.length(); i++) {
                char currChar = word.charAt(i);
                if ((currChar >= 'A') && (currChar <= 'Z')) {
                    ans = false;
                    break;
                };
            };
        };
        return ans;
    }
}
```





