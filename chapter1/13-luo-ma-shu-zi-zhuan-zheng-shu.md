# 13 罗马数字转整数

```text
罗马数字包含以下七种字符: I， V， X， L，C，D 和 M。 
| 字符 |     数值   | 
| I    |     1     | 
| V    |     5     | 
| X    |    10     | 
| L    |    50     | 
| C    |    100    | 
| D    |    500    | 
| M    |    1000   |  

例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做 XXVII, 即为 XX + V + II 。 

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。
数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。

同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况： 
1)I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。 
2)X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
3)C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。 

🍓 题目：给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。 
示例 1:      输入: "III"     输出: 3 
示例 2:      输入: "IV"      输出: 4 
示例 3:      输入: "IX"      输出: 9 
示例 4:      输入: "LVIII"   输出: 58        解释: L = 50, V= 5, III = 3.
示例 5:      输入: "MCMXCIV" 输出: 1994      解释: M = 1000, CM = 900, XC = 90, IV = 4. 

Related Topics 数学 字符串
```

## 思路：

* 通常情况下，罗马数字中小的数字在大的数字的右边 --&gt; 直接针对字符对应的数据相加即可
* 罗马数字中小的数字在大的数字的左边 --&gt; 相邻的两个元素是关联的，用右侧大数-左侧小数
* 因此，只要判断当前字符与右侧字符之间的关系，正确运算即可。

### 实现一、使用 MAP 存储对应关系

```java
class Solution {
    public int romanToInt(String s) {
        HashMap<Character, Integer> map = new HashMap<>();
        map.put('I',1);
        map.put('V',5);
        map.put('X',10);
        map.put('L',50);
        map.put('C',100);
        map.put('D',500);
        map.put('M',1000);

        int res = 0;
        for(int i=0; i<s.length();i++){
            if(i!=s.length()-1 && map.get(s.charAt(i)) < map.get(s.charAt(i+1))){
                res += (map.get(s.charAt(i+1)) - map.get(s.charAt(i)));
                i++;
            }else{
                res +=  map.get(s.charAt(i));
            }

        }
        return res;

    }
}
```

### 实现二、使用 int 数组存储 map 对应关系

使用数组替代 map，因为只有几个字符的对应关系，而字符与 ASII 码相对应，可以当作整数数组坐标

```java
class Solution {
    public int romanToInt(String s) {
        int[] map = new int[128];   //要指定数组大小
        map['I']=1;
        map['V']=5;
        map['X']=10;
        map['L']=50;
        map['C']=100;
        map['D']=500;
        map['M']=1000;

        int res = 0;
        for(int i=0; i<s.length(); i++){

            if(i!= s.length()-1  &&  map[s.charAt(i)] < map[s.charAt(i+1)]){
                res += (map[s.charAt(i+1)] - map[s.charAt(i)]);
                i++;
            }else{
                res += map[s.charAt(i)];
            }
        }
        return res;
    }
}
```

