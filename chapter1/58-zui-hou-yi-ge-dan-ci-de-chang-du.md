## 58. 最后一个单词的长度



```
给定一个仅包含大小写字母和空格 ' ' 的字符串 s，返回其最后一个单词的长度。如果字符串从左向右滚动显示，那么最后一个单词就是最后出现的单词。 

如果不存在最后一个单词，请返回 0 。 

说明：一个单词是指仅由字母组成、不包含任何空格字符的最大子字符串。 

示例: 
输入: "Hello World"
输出: 5

Related Topics 字符串 
```

### 方法一、从后向前遍历

从字符串末尾开始向前遍历，其中主要有两种情况
* 第一种情况，以字符串"Hello World"为例，从后向前遍历直到遍历到头或者遇到空格为止，即为最后一个单词"World"的长度5
* 第二种情况，以字符串"Hello World "为例，需要先将末尾的空格过滤掉，再进行第一种情况的操作，即认为最后一个单词为"World"，长度为5

所以完整过程为先从后过滤掉空格找到单词尾部，再从尾部向前遍历，找到单词头部，最后两者相减，即为单词的长度


```java
class Solution {
    public int lengthOfLastWord(String s) {
        int end = s.length()-1;
        while(end>=0 && s.charAt(end)==' '){
            //指向最后一个非空格字符
            end--;
        }
        if(end<0){
            return 0;
        }
        int start = end;
        while(start>=0 && s.charAt(start)!=' '){
            start--;
        }

        return end-start;
    }
}
```

### 方法二、lastIndexOf寻找空格位置

思路：使用 trim 函数去除前后多余空格，然后用 lastIndexOf 函数找到最后一个空格位置。

```java
class Solution {
    public int lengthOfLastWord(String s) {
        s = s.trim();
        int first = s.lastIndexOf(" ");
        if(first == -1) //没有找到空格，只有一个单词
            return s.length();
        return s.length()-first-1;
    }
}
```


### 方法三、split+正则表达式

【不推荐】

```java
class Solution {
    public int lengthOfLastWord(String s) {
        s = s.trim();
        String[] splitStr=s.split("\\s");
        int len = splitStr.length;
        if( len > 0){
            return splitStr[len-1].length();
        }else{
            return 0;
        }
    }
}
```


