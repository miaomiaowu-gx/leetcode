## 383. 赎金信

```
给定一个赎金信 (ransom) 字符串和一个杂志(magazine)字符串，判断第一个字符串 ransom 能不能由第二个字符串 magazines 里面的字符构成。如果可以构成，返回 true ；否则返回 false。 
(题目说明：为了不暴露赎金信字迹，要从杂志上搜索各个需要的字母，组成单词来表达意思。杂志字符串中的每个字符只能在赎金信字符串中使用一次。) 

注意： 
你可以假设两个字符串均【只含有小写字母】。 
canConstruct("a", "b") -> false
canConstruct("aa", "ab") -> false
canConstruct("aa", "aab") -> true

Related Topics 字符串 
```


### 方法一、使用 indexOf 函数

`java.lang.String` 中方法 `int indexOf(int ch, int fromIndex)` 返回在此字符串中第一次出现指定字符处的索引，从指定的索引开始搜索。

1. 判定 magazine 的长度是否小于 ransom，如果小于那么一定是 false。
2. 遍历 ransom 当中的所有字符，caps 中保存从 magazine 搜索的起始位置。

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        if(ransomNote.length() > magazine.length()) return false;
        int caps[] = new int[26];
        for(char c: ransomNote.toCharArray()){
            int index = magazine.indexOf(c, caps[c-'a']);
            if(index == -1) return false;
            caps[c-'a'] = index+1;
        }
        return true;
    }
}
```

最坏时间复杂度是 m*n，m 是 ransom 去重后的长度，n 是 magazine 长度。比普通的哈希法的线性复杂度要高。


### 方法二、数组代替哈希表计数

【推荐】


```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        int b[]=new int[26];
        for(char c:magazine.toCharArray()){
            b[c-'a']++;
        }
        for(char c:ransomNote.toCharArray()){
            if(b[c-'a']==0)
                return false;
            b[c-'a']--;
        }
        return true;
    }
}
```

### 方法三、哈希表计数

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        Map<Character, Integer> mag = new HashMap<>();
        for(char ch: magazine.toCharArray()){
            if(mag.containsKey(ch)) mag.put(ch, mag.get(ch)+1);
            else mag.put(ch, 1);
        }
        for(char ch: ransomNote.toCharArray()){
            if(!mag.containsKey(ch)) return false;//如果赎金信里面的字母，杂志中没有
            else if(mag.get(ch)==0) return false;//如果杂志中的字符不够
            else mag.put(ch, mag.get(ch)-1);
        }
        return true;
    }
}
```
