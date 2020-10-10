## 67. 二进制求和



```
给你两个二进制字符串，返回它们的和（用二进制表示）。输入为 非空 字符串且只包含数字 1 和 0。 

示例 1: 
输入: a = "11", b = "1"
输出: "100" 

示例 2: 
输入: a = "1010", b = "1011"
输出: "10101" 

提示： 
每个字符串仅由字符 '0' 或 '1' 组成。 
1 <= a.length, b.length <= 10^4 
字符串如果不是 "0" ，就都不含前导零。 

Related Topics 数学 字符串 
```


### 方法一、Java语言自带高精度运算

【不能通过，不推荐】

先将 a 和 b 转化成十进制数，求和后再转化为二进制数。

```java
class Solution {
    public String addBinary(String a, String b) {
        return Integer.toBinaryString(
                Integer.parseInt(a, 2) + Integer.parseInt(b, 2)
        );
    }
}
```

`parseInt(String s,int radix)`就是求 `radix` 进制数 `s` 的十进制数是多少。

**报错原因：**

在 Java 中：

* 如果字符串超过 `3333` 位，不能转化为 `Integer`
* 如果字符串超过 `6565` 位，不能转化为 `Long`
* 如果字符串超过 `500000001500000001` 位，不能转化为 `BigInteger`

因此，为了适用于长度较大的字符串计算，应该使用更加健壮的算法。


### 方法二、模拟

思路：借鉴「列竖式」的方法，末尾对齐，逐位相加，「逢二进一」。

1. 取$$n=max{\left(|a|,|b|\right)}$$，循环 n 次，从最低位开始遍历。使用一个变量 carry 表示上一个位置的进位，初始值为 0。
2. 记当前位置对其的两个位为 $$a_i$$ 与 $$b_i$$，则每一位的答案为 $$carry + a_i + b_i$$，下一位的进位为 $$(carry + a_i + b_i)/2$$。
3. 重复上述步骤，直到数字 a 和 b 的每一位计算完毕。最后如果 carry 的最高位不为 0，则将最高位添加到计算结果的末尾。



```java
class Solution {
    public String addBinary(String a, String b) {
        StringBuffer sb = new StringBuffer();
        int length = Math.max(a.length(),b.length());
        int carry = 0;
        for(int i=0; i<length; i++){
            //从后向前遍历字符
            carry += i<a.length()?a.charAt(a.length()-1-i)-'0':0;
            carry += i<b.length()?b.charAt(b.length()-1-i)-'0':0;
            sb.append((char)(carry%2+'0'));  //将整数存为对应字符串
            carry /=2;
        }
        if(carry>0) sb.append("1");
        sb.reverse();
        return sb.toString();
    }
}
```


**复杂度分析**
* 时间复杂度：O(n)，这里的时间复杂度来源于顺序遍历 a 和 b。
* 空间复杂度：O(1)，除去答案所占用的空间，这里使用了常数个临时变量。



### 方法三、位运算


使用**位运算**替代上述运算中的一些**加减乘除**的操作。

把 a 和 b 转换成整型数字 x 和 y，在接下来的过程中，x 保存结果，y 保存进位。
当进位不为 0 时
计算当前 x 和 y 的无进位相加结果：`answer = x ^ y`
计算当前 x 和 y 的进位：`carry = (x & y) << 1`
完成本次循环，更新 `x = answer，y = carry`

```python
class Solution:
    def addBinary(self, a, b) -> str:
        x, y = int(a, 2), int(b, 2)
        while y:
            answer = x ^ y
            carry = (x & y) << 1
            x, y = answer, carry
        return bin(x)[2:]
```







