# 557 反转字符串中的单词 III

```text
给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。 

示例： 
输入："Let's take LeetCode contest"
输出："s'teL ekat edoCteeL tsetnoc"

提示： 
在字符串中，每个单词由【单个空格分隔】，并且字符串中不会有任何额外的空格。 

Related Topics 字符串
```

## 方法一、使用额外空间

算法：开辟一个新字符串。然后从头到尾遍历原字符串，直到找到空格为止，此时找到了一个单词，并能得到单词的起止位置。随后，根据单词的起止位置，可以将该单词逆序放到新字符串当中。如此循环多次，直到遍历完原字符串，就能得到翻转后的结果。

```java
class Solution {
    public String reverseWords(String s) {
        StringBuffer sb = new StringBuffer();
        int len = s.length();
        int i=0;
        while(i<len){
            int start = i;
            while(i<len && s.charAt(i)!=' ') i++;
            for(int p=i-1; p>=start && p>=0 && p<len; p--){
                sb.append(s.charAt(p));
            }
            if(i<len && s.charAt(i)==' '){
                sb.append(' ');
                i++;
            }
        }
        return sb.toString();
    }
}
```

**复杂度分析**

* 时间复杂度：O\(N\)，其中 N 为字符串的长度。原字符串中的每个字符都会在 O\(1\) 的时间内放入新字符串中。
* 空间复杂度：O\(N\)。开辟了与原字符串等大的空间。

## 方法二、使用 split 函数

```java
class Solution {
    public String reverseWords(String s) {
        String[] current = s.split(" ");
        StringBuilder result =new StringBuilder();
        for (String word : current)
        {
            result.append(new StringBuffer(word).reverse().toString()).append(" ");
        }
        return result.toString().trim();
    }
}
```

