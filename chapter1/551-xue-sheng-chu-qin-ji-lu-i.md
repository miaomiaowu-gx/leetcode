# 551 学生出勤记录I

```java
给定一个字符串来代表一个学生的出勤记录，这个记录仅包含以下三个字符： 
'A' : Absent，缺勤 
'L' : Late，迟到 
'P' : Present，到场 

如果一个学生的出勤记录中不超过一个'A'(缺勤)并且不超过两个【连续】的'L'(迟到)，那么这个学生会被奖赏。 

你需要根据这个学生的出勤记录判断他是否会被奖赏。 

示例 1: 
输入: "PPALLP"
输出: True
示例 2: 
输入: "PPALLL"
输出: False

Related Topics 字符串
```

## 方法一、利用 indexof 函数

Java 中 indexOf 方法可以用来检查一个串是否是另一个串的子串。如果找不到子串，那么返回 -1，否则返回这个字符串第一次出现的位置。

算法思路：统计字符串中 A 的数目并检查 LLL 是否是给定字符串的一个子串。

```java
class Solution {
    public boolean checkRecord(String s) {
        int len = s.length(), countA = 0;
        //当 A 多余两个时提前结束循环
        for(int i=0;i<len  && count<2;i++){
            if(s.charAt(i)=='A')  countA++;
        }
        return countA<2 && s.indexOf("LLL")<0;
    }
}
```

## 方法二、不需要 indexOf 的单遍循环方法

```java
class Solution {
    public boolean checkRecord(String s) {
        int len = s.length(), countA = 0;
        for(int i=0;i<len;i++){
            if(s.charAt(i)=='A')  countA++;
            if(i<=len-3 && s.charAt(i)=='L' && s.charAt(i+1)=='L' && s.charAt(i+2)=='L')
                return false;
        }
        return countA<2;
    }
}
```

**复杂度分析**

* 时间复杂度： O\(n\)。单遍循环的上限是字符串的长度。
* 空间复杂度： O\(1\)。只使用了常数级别的空间。

## 方法三、使用正则表达式

正则表达式符号：

```text
. : 匹配任何除了换行以外的单个字符。
* : 匹配 0 个或者 大于 0 个 * 符号前面的表达式。
.* : 匹配任何字符串
a|b : 要么匹配 a 要么匹配 b
```

matches 方法被用来检查是否有字符串匹配我们给定的正则表达式。

包含 2 个或更多 AA 的正则表达式为 `.*A.*A.*` ，包含子字符串 LLL 对应的正则表达式为 `.*LLL.*`。可以把两个正则表达式用 \| 形成一个正则表达式，来表示或者包含超过 1 个 A 或者包含 3 个连续的 L 的字符串。那么正则表达就变成 `.*(A.*A|LLL).*`。只有当字符串不能匹配正则表达式的时候我们返回 true。

```java
class Solution {
    public boolean checkRecord(String s) {
        return !s.matches(".*(A.*A|LLL).*");
    }
}
```

