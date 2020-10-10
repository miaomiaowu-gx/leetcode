# 788 旋转数字

、\#\# 788. 旋转数字

```text
称一个数 X 为好数, 如果它的每位数字逐个地被旋转 180 度后，我们仍可以得到一个有效的，且和 X 不同的数。要求每位数字都要被旋转。 
如果一个数的每位数字被旋转以后仍然还是一个数字，则这个数是有效的。0, 1, 和 8 被旋转后仍然是它们自己；2 和 5 可以互相旋转成对方（在这种情况下，它们以不同的方向旋转，换句话说，2 和 5 互为镜像）；6 和 9 同理，除了这些以外其他的数字旋转以后都不再是有效的数字。 
现在我们有一个正整数 N, 计算从 1 到 N 中有多少个数 X 是好数？ 

示例： 
输入: 10
输出: 4
解释: 在[1, 10]中有四个好数： 2, 5, 6, 9。
注意 1 和 10 不是好数, 因为他们在旋转之后不变。

提示： N 的取值范围是 [1, 10000]。 

Related Topics 字符串
```

【推荐】

## 方法一、暴力方法

思路：遍历从 1 到 N 的每个数字 X，判断 X 是否为好数。

* 如果 X 中存在 3、4、7 这样的无效数字，则 X 不是一个好数。
* 如果 X 中不存在 2、5、6、9 这样的旋转后会变成不同的数字，则 X 不是一个好数。
* 否则，X 可以旋转成一个不同的有效数字。

递归检查 X 的最后一位数字，flag的使用很巧妙。

```java
class Solution {
    public int rotatedDigits(int N) {
        int ans = 0;
        for(int n=1; n<=N; ++n){
            if(good(n, false)) ans++;
        }
        return ans;
    }

    //The flag is true if we have an occurrence of 2, 5, 6, 9.
    public boolean good(int n, boolean flag){
        if(n==0) return flag;
        int d = n % 10;
        if(d == 3 || d == 4 || d == 7) return false;
        if(d == 0 || d == 1 || d == 8) return good(n/10, flag);
        return good(n/10, true);
    }
}
```

**复杂度分析**

* 时间复杂度：O\(NlogN\)，检查每个 X 的每一位数字。
* 空间复杂度：O\(logN\)，存储字符串或者 good 函数的调用栈。

## 方法二、动态规划

[https://leetcode-cn.com/problems/rotated-digits/solution/xuan-zhuan-shu-zi-by-leetcode/](https://leetcode-cn.com/problems/rotated-digits/solution/xuan-zhuan-shu-zi-by-leetcode/)

