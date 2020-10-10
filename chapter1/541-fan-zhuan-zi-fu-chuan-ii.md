# 541 反转字符串II

```text
给定一个字符串 s 和一个整数 k，你需要对从字符串开头算起的每隔 2k 个字符的前 k 个字符进行反转。 

如果剩余字符少于 k 个，则将剩余字符全部反转。 
如果剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符，其余字符保持原样。 

示例: 
输入: s = "abcdefg", k = 2
输出: "bacdfeg"

提示： 
该字符串只包含小写英文字母。 
给定字符串的长度和 k 在 [1, 10000] 范围内。 

Related Topics 字符串
```

## 直接翻转

```java
class Solution {
    public String reverseStr(String s, int k) {
        int len = s.length();
        char arr[] = s.toCharArray();

        for(int i=0;i<len;i = i+2*k){
            int start  = i;
            int end = Math.min(i+k,len)-1;
            while(start<end){
                char c =arr[start];
                arr[start] = arr[end];
                arr[end] = c;
                start++;
                end--;
            }
        }

        return new String(arr);
    }
}
```

**复杂度分析**

* 时间复杂度：O\(N\)，其中 N 是 s 的大小。建立一个辅助数组，用来翻转 s 的一半字符。
* 空间复杂度：O\(N\)，a 的大小。

