# 387 字符串中的第一个唯一字符

```text
给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。 

示例： 
s = "leetcode"
返回 0
s = "loveleetcode"
返回 2

提示：你可以假定该字符串【只包含小写字母】。 

Related Topics 哈希表 字符串
```

算法： 1. 遍历一遍字符串，把字符串中每个字符出现的次数保存在一个散列表中。这个过程的时间复杂度为 O\(N\)，其中 N 为字符串的长度。 2. 再遍历一次字符串，这一次利用散列表来检查遍历的每个字符是不是唯一的。如果当前字符唯一，直接返回当前下标就可以了。第二次遍历的时间复杂度也是 O\(N\)。

## 方法一、数组代替哈希表计数

```java
class Solution {
    public int firstUniqChar(String s) {
        int arr[] = new int[26];
        for(char c: s.toCharArray()){
            arr[c-'a']++;
        }
        int count = 0;
        for(char c: s.toCharArray()){
            if(arr[c-'a']==1) return count;
            count++;
        }
        return -1;
    }
}
```

## 方法二、哈希表计数

```java
class Solution {
    public int firstUniqChar(String s) {
        HashMap<Character, Integer> count = new HashMap<Character, Integer>();
        int n = s.length();
        // build hash map : character and how often it appears
        for (int i = 0; i < n; i++) {
            char c = s.charAt(i);
            count.put(c, count.getOrDefault(c, 0) + 1);
        }

        // find the index
        for (int i = 0; i < n; i++) {
            if (count.get(s.charAt(i)) == 1)
                return i;
        }
        return -1;
    }
}
```

