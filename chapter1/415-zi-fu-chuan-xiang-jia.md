## 415. 字符串相加

```
给定两个字符串形式的【非负】整数 num1 和num2 ，计算它们的和。 

提示： 
num1 和 num2 的长度都小于 5100 
num1 和 num2 都只包含数字 0-9 
num1 和 num2 都不包含任何前导零 
你不能使用任何內建 BigInteger 库， 也不能直接将输入的字符串转换为整数形式 

Related Topics 字符串 
```


### 方法一、模拟「竖式加法」


```java
class Solution {
    public String addStrings(String num1, String num2) {
        int i = num1.length()-1, j = num2.length()-1, add = 0;
        StringBuffer ans = new StringBuffer();
        while(i>=0 || j>=0 || add>0){
            int x = (i>=0)? num1.charAt(i)-'0':0;
            int y = (j>=0)? num2.charAt(j)-'0':0;
            int result = x + y + add;
            ans.append(result % 10);
            add = result/10;
            j--;
            i--;
        }
        return ans.reverse().toString();
    }
}
```

**复杂度分析**

* 时间复杂度：O(max(len1,len2))，其中len1=num1.length，len2=num2.length。竖式加法的次数取决于较大数的位数。
* 空间复杂度：O(1)。除答案外只需要常数空间存放若干变量。在 Java 解法中使用到了 StringBuffer，故 Java 解法的空间复杂度为 O(n)。



