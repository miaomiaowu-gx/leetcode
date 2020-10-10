## 28. 实现strStr()

```
实现 strStr() 函数。 
给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回 -1。 

示例 1: 
输入: haystack = "hello", needle = "ll"
输出: 2

示例 2: 
输入: haystack = "aaaaa", needle = "bba"
输出: -1

说明: 
当 needle 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。 
对于本题而言，当 needle 是空字符串时我们应当返回 0 。这与C语言的 strstr() 以及 Java的 indexOf() 定义相符。 

Related Topics 双指针 字符串 
```


### 方法一、子串逐一比较 - [不推荐]

从 haystack 字符串头部开始，不断截取与 needle 字符串等长的子字符串，并与 needle 字符串比较。

```java
class Solution {
    public int strStr(String haystack, String needle) {
        //if(needle =="") return 0;
        int L = needle.length(), n = haystack.length();

        for (int start = 0; start < n - L + 1; ++start) {
            if (haystack.substring(start, start + L).equals(needle)) {
                return start;
            }
        }
        return -1;
    }
}
```
**缺陷**：会将 haystack 所有长度为 L 的子串都与 needle 字符串比较


**复杂度分析**

* 时间复杂度：O((N - L)L)，其中 N 为 haystack 字符串的长度，L 为 needle 字符串的长度。内循环中比较字符串的复杂度为 L，总共需要比较 (N - L) 次。

* 空间复杂度：O(1)。



### 方法二、双指针 - [一般]

只有子串的第一个字符跟 needle 字符串第一个字符**相同**的时候才需要比较。

双指针：pn 指向 haystack 字符串，pL 指向 needle 字符串，curr_len 为当前匹配的长度。

* 移动 pn 指针，直到 pn 所指向位置的字符与 needle 字符串第一个字符相等。

* 逐步向后移动指针 pn，pL，如果匹配，则增加 curr_len，不匹配则停止。

   * 如果完全匹配（即 curr_len == L），返回匹配子串的起始坐标（即 pn - L）。

   * 如果不完全匹配，回溯。使 pn = pn - curr_len + 1， pL = 0， curr_len = 0。

```java
class Solution {
  public int strStr(String haystack, String needle) {
    int L = needle.length(), n = haystack.length();
    if (L == 0) return 0;

    int pn = 0;
    while (pn < n - L + 1) {

      while (pn < n - L + 1 && haystack.charAt(pn) != needle.charAt(0)) ++pn;

      // compute the max match string
      int currLen = 0, pL = 0;
      while (pL < L && pn < n && haystack.charAt(pn) == needle.charAt(pL)) {
        ++pn;
        ++pL;
        ++currLen;
      }

      if (currLen == L) return pn - L;

      // otherwise, backtrack
      pn = pn - currLen + 1;
    }
    return -1;
  }
}
```

**复杂度分析**

* 时间复杂度：最坏时间复杂度为 O((N - L)L)，最优时间复杂度为 O(N)。

* 空间复杂度：O(1)。


### 方法三、 Rabin Karp - [🍓常数复杂度🍓]

思路：先生成窗口内子串的哈希码，然后再跟 needle 字符串的哈希码做比较。因此问题转化为如何在常数时间生成子串的哈希码。

**滚动哈希：常数时间生成哈希码**

常数时间生成滑动窗口数组的哈希码：利用滑动窗口的特性，每次滑动都有一个元素进，一个出。

由于只会出现小写的英文字母，因此可以将字符串转化成值为 0 到 25 的整数数组：`arr[i] = (int)S.charAt(i) - (int)'a'`。按照这种规则，`abcd` 整数数组形式就是 `[0, 1, 2, 3]`，转换公式如下所示。


$$
h_0 =0×26^3
 +1×26^2
 +2×26^1
 +3×26^0
$$

可以将上面的公式写成通式，如下所示。其中 $$c_i$$ 为整数数组中的元素，`a = 26`，其为字符集的个数。



$$
h_0 = c_0a^{L−1} + c_1a^{L−2} +...+ c_ia^{L−1−i} +...+ c_{L−1}a^0 
$$

下面来考虑窗口从 `abcd` 滑动到 `bcde` 的情况。这时候整数形式数组从 `[0, 1, 2, 3]` 变成了 `[1, 2, 3, 4]`，数组最左边的 `0` 被移除，同时最右边新添了 `4`。滑动后数组的哈希值可以根据滑动前数组的哈希值来计算，计算公式如下所示。



$$
h_1 = (h_0 − 0×26^3)×26+4×26^0
$$

写成通式如下所示:



$$
h_1 = (h_0a - c_0a^L) + c_L
$$

 

**如何避免溢出**

$$a^L$$ 可能是一个很大的数字，因此需要设置数值上限来避免溢出。设置数值上限可以用取模的方式，即用 `h % modulus` 来代替原本的哈希值。取 $$ modulus = 2^{31} $$。



**算法**

* 计算子字符串 `haystack.substring(0, L)` 和 `needle.substring(0, L)` 的哈希值。

* 从起始位置开始遍历：从第一个字符遍历到第 N - L 个字符。

   * 根据前一个哈希值计算滚动哈希。

   * 如果子字符串哈希值与 needle 字符串哈希值相等，返回滑动窗口起始位置。

* 返回 -1，这时候 haystack 字符串中不存在 needle 字符串。



```java
class Solution {

    public int charToInt(int idx, String s){
        return (int)s.charAt(idx) - (int)'a';
    }

    public int strStr(String haystack, String needle) {
        int L = needle.length(), n = haystack.length();
        if(L>n) return -1;

        int a = 26;
        long modulus = (long)Math.pow(2,31);

        long h = 0, ref_h = 0;
        long aL = 1;
        for(int i=0; i<L; i++){
            h = (h * a + charToInt(i, haystack)) % modulus;
            ref_h = (ref_h * a + charToInt(i, needle)) % modulus;
            aL = (aL * a) % modulus;
        }

        if(h == ref_h) return 0;

        for(int start = 1; start<n-L+1; start++){
            h = (h *a - charToInt(start - 1, haystack)* aL + charToInt(start+L-1, haystack))%modulus;
            if(h == ref_h) return start;
        }

        return -1;

    }
}

```





**复杂度分析**

* 时间复杂度：O(N)，计算 needle 字符串的哈希值需要 O(L) 时间，之后需要执行 (N - L) 次循环，每次循环的计算复杂度为常数。

*空间复杂度：O(1)。






