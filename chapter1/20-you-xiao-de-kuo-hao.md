# 20 有效的括号

```text
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。 

有效字符串需满足： 
- 左括号必须用相同类型的右括号闭合。 
- 左括号必须以正确的顺序闭合。 
- 注意空字符串可被认为是有效字符串。 

示例 1:              输入: "()"            输出: true
示例 2:              输入: "()[]{}"        输出: true
示例 3:              输入: "(]"            输出: false
示例 4:              输入: "([)]"          输出: false
示例 5:              输入: "{[]}"          输出: true 

Related Topics 栈 字符串
```

## 方法一、使用栈

思路：

遍历输入字符串

1. 当前字符是左括号时，入栈；
2. 当前字符是右括号时，观察栈顶元素
   * 栈顶元素为与当前右括号匹配的左括号，弹出左括号，继续遍历字符串（说明两者相匹配）。
   * 栈顶元素不是当前右括号匹配的左括号，返回false（括号不匹配）。

### 实现 1 栈+Hashmap实现

```java
class Solution {

    public boolean isValid(String s) {
        if(s.length()%2==1){
            return false;
        }

        HashMap<Character, Character> map = new HashMap<>();
        map.put(')', '(');
        map.put(']', '[');
        map.put('}', '{');

        Stack<Character> stack = new Stack<>();

        for(int i=0; i<s.length(); i++){
            char ch = s.charAt(i);
            if(ch == '(' || ch == '[' || ch == '{'){
                stack.push(ch);
            }else if(!stack.empty() && stack.peek() == map.get(ch)){
                stack.pop();
            }else{
                return false;
            }
        }

        return stack.empty();

    }
}
```

### 实现 2 栈实现

```java
class Solution {
    public boolean isValid(String s) {
        if(s.isEmpty())
            return true;
        Stack<Character> stack=new Stack<Character>();
        for(char c:s.toCharArray()){
            if(c=='(')
                stack.push(')');
            else if(c=='{')
                stack.push('}');
            else if(c=='[')
                stack.push(']');
            else if(stack.empty()||c!=stack.pop())
                return false;
        }
        return stack.empty();
    }
}
```

## 方法二、字符串替换

**\[不推荐\]** 复杂度高，仅作为了解。时间复杂度是O\(n^2\)。

```java
public boolean isValid(String s) {
        if (s.contains("()") || s.contains("[]") || s.contains("{}")) {
            return isValid(s.replace("()", "").replace("[]", "").replace("{}", ""));
        } else {
            return "".equals(s);
        }
}
```

