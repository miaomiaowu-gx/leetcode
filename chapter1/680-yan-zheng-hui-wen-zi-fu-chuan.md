# 680 验证回文字符串 Ⅱ

```text
给定一个非空字符串 s，最多删除一个字符。判断是否能成为回文字符串。 

示例 1: 
输入: "aba"
输出: True

示例 2: 
输入: "abca"
输出: True
解释: 你可以删除c字符。

注意: 
字符串只包含从 a-z 的小写字母。字符串的最大长度是50000。 

Related Topics 字符串
```

## 方法：贪心算法

算法思路：使用双指针，初始化两个指针 low 和 high 分别指向字符串的第一个字符和最后一个字符。每次判断两个指针指向的字符是否相同，

* 如果相同，则更新指针，令 low = low + 1 和 high = high - 1，然后判断更新后的指针范围内的子串是否是回文字符串。
* 如果两个指针指向的字符不同，则两个字符中必须有一个被删除，此时就分成两种情况：即删除左指针对应的字符，留下子串 s\[low+1, high\]，删除右指针对应的字符，留下子串 s\[low, high-1\]。当这两个子串中至少有一个是回文串时，就说明原始字符串删除一个字符之后就以成为回文串。

```java
class Solution {
    public boolean validPalindrome(String s) {
        int low = 0, high = s.length()-1;
        while (low<high){
            char c1 = s.charAt(low), c2 = s.charAt(high);
            if(c1==c2){
                low++;
                high--;
            }else{
                boolean flag1 = true, flag2 = true;
                for(int i=low, j=high-1; i<j; i++,j--){
                    if(s.charAt(i)!=s.charAt(j)){
                        flag1 = false;
                        break;
                    }
                }
                for(int i=low+1, j=high; i<j; i++,j--){
                    if(s.charAt(i)!=s.charAt(j)){
                        flag2 = false;
                        break;
                    }
                }
                return flag1 || flag2;
            }
        }
        return true;
    }
}
```

```java
class Solution {
    public boolean validPalindrome(String s) {
        for(int i = 0, j = s.length()-1; i < j ; i++, j--){
            if(s.charAt(i) != s.charAt(j)){
                //分两种情况，一是右边减一，二是左边加一
                return isPalindrome(s,i,j-1) || isPalindrome(s, i+1, j);
            }
        }
        return true;
    }

    public boolean isPalindrome(String s, int i, int j) {
        while (i < j) {
            if (s.charAt(i++) != s.charAt(j--)) {
                return false;
            }
        }
        return true;
    }
}
```

