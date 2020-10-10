## 345. 反转字符串中的元音字母

```
编写一个函数，以字符串作为输入，反转该字符串中的元音字母。 

示例 1： 
输入："hello"
输出："holle"

示例 2： 
输入："leetcode"
输出："leotcede" 

提示： 元音字母不包含字母 "y" 。 

Related Topics 双指针 字符串 
```

### 方法、双指针法

一般遇见字符串问题，能转成字符数组就尽量转(方便)；

转换成数组后，分别定义前后两个索引指针用 while 依次遍历数组；

```java
class Solution {
    public String reverseVowels(String s) {

        char[] arr = s.toCharArray();

        //元音字母要考虑大小写！
        HashSet<Character> set = new HashSet<>();
        set.add('a');
        set.add('e');
        set.add('i');
        set.add('o');
        set.add('u');
        set.add('A');
        set.add('E');
        set.add('I');
        set.add('O');
        set.add('U');
        int left = 0, right = s.length()-1;
        while(left<right){
            while(left<right && !set.contains(arr[left])){
                left++;
            }
            while(left<right && !set.contains(arr[right])){
                right--;
            }

            char t = arr[left];
            arr[left] = arr[right];
            arr[right] = t;
            left++;
            right--;
        }
        //s = arr.toString(); 错误
        //return s;
        //String res = arr.toString(); //错误
        //return res; //输出"[C@23ab930d"
        return new String(arr); //必须要用arr
    }
}
```

注意：由于 String 是引用类型，用 new String。



