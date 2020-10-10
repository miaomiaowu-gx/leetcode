## 344. 反转字符串

```
编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 char[] 的形式给出。 
不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。 
你可以假设数组中的所有字符都是 ASCII 码表中的可打印字符。 
示例 1： 
输入：["h","e","l","l","o"]
输出：["o","l","l","e","h"]
示例 2： 
输入：["H","a","n","n","a","h"]
输出：["h","a","n","n","a","H"] 
Related Topics 双指针 字符串 
```

### 方法一、双指针法

思路：双指针法是使用两个指针，一个左指针 start，右指针 end，开始工作时 start 指向首元素，end 指向尾元素。交换两个指针指向的元素，并向中间移动，直到两个指针相遇。

```java
class Solution {
    public void reverseString(char[] s) {
        int len = s.length;
        int start = 0, end = len-1;
        while(start<end){
            char t = s[start];
            s[start] = s[end];
            s[end] = t;
            start++;
            end--;
        }
        return ;
    }
}
```

**复杂度分析**

* 时间复杂度：O(N)。执行了 N/2 次的交换。
* 空间复杂度 O(1)，只使用了常数级空间。



### 方法二、递归


使用递归的方法去反转字符串，它是原地反转，但是空间复杂度却不是常数级空间，因为递归过程中使用了堆栈空间。

```java
class Solution {
  public void helper(char[] s, int left, int right) {
    if (left >= right) return;
    char tmp = s[left];
    s[left++] = s[right];
    s[right--] = tmp;
    helper(s, left, right);
  }

  public void reverseString(char[] s) {
    helper(s, 0, s.length - 1);
  }
}
```


**复杂度分析**

* 时间复杂度：O(N)。执行了 N/2 次的交换。
* 空间复杂度：O(N)，递归过程中使用的堆栈空间。









