# 125 验证回文串

```text
给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。 

说明：本题中，我们将空字符串定义为有效的回文串。 

示例 1: 
输入: "A man, a plan, a canal: Panama"
输出: true

示例 2: 
输入: "race a car"
输出: false

Related Topics 双指针 字符串
```

## 方法一、在原字符串上使用双指针

思路：在原字符串 s 上使用双指针。两指针分别从头尾部向中间移动，直到遇到一个字母或数字字符，或者两指针重合为止。即每次将指针移到下一个字母字符或数字字符，再判断这两个指针指向的字符是否相同。

```java
class Solution {
    public boolean isPalindrome(String s) {
        s = s.toLowerCase();
        int start = 0, end = s.length()-1;
        while(start<=end){
            while(start < end && !Character.isLetterOrDigit(s.charAt(start))){
                start++;
            }
            while(start < end && !Character.isLetterOrDigit(s.charAt(end))){
                end--;
            }
            if(s.charAt(start)!=s.charAt(end)){
                return false;
            }
            start++;
            end--;
        }
        return true;
    }
}
```

**复杂度分析**

* 时间复杂度：O\(∣s∣\)，其中 ∣s∣ 是字符串 s 的长度。
* 空间复杂度：O\(1\)。

## 方法二、筛选 + 判断

思路：

1. 对字符串 s 进行一次遍历，并将其中的字母和数字字符进行保留，放在另一个字符串 sgood 中。
2. 使用语言中的字符串翻转 API 得到 sgood 的逆序字符串 sgood\_rev 判断这两个字符串是否相同。

```java
class Solution {
    public boolean isPalindrome(String s) {
        StringBuffer sgood = new StringBuffer();
        int length = s.length();
        for (int i = 0; i < length; i++) {
            char ch = s.charAt(i);
            if (Character.isLetterOrDigit(ch)) {
                sgood.append(Character.toLowerCase(ch));
            }
        }
        StringBuffer sgood_rev = new StringBuffer(sgood).reverse();
        return sgood.toString().equals(sgood_rev.toString());
    }
}
```

