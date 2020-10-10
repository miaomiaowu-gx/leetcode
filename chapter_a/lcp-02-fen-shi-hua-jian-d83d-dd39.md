## LCP 02 分式化简

```
有一个同学在学习分式。他需要将一个连分数化成最简分数，你能帮助他吗？ 
```

<img src="./img_a/fraction_example_1.jpg" width=100>

```
连分数是形如上图的分式。在本题中，所有系数都是【大于等于0的整数】。 

输入的cont代表连分数的系数（cont[0]代表上图的a0，以此类推）。返回一个长度为2的数组[n, m]，使得连分数的值等于n/m，且n, m最大公约数为1。 

示例 1： 
输入：cont = [3, 2, 0, 2]
输出：[13, 4]
解释：原连分数等价于3 + (1 / (2 + (1 / (0 + 1 / 2))))。注意[26, 8], [-13, -4]都不是正确答案。 

示例 2： 
输入：cont = [0, 0, 3]
输出：[3, 1]
解释：如果答案是整数，令分母为1即可。 

限制： 
cont[i] >= 0 
1 <= cont的长度 <= 10 
cont最后一个元素不等于0 
答案的n, m的取值都能被32位int整型存下（即不超过2 ^ 31 - 1）。 
```

### 方法一、迭代模拟运算过程

```java
class Solution {
    public int[] fraction(int[] cont) {
        int[] res = new int[2];
        res[0] = cont[cont.length-1];
        res[1] = 1;
        for(int i = cont.length-1; i>0; i-- ){
            int temp = res[0];
            res[0] = cont[i-1]*res[0]+res[1];
            res[1] = temp;
        }
        return res;
    }
}
```



### 方法二、递归

```java
class Solution {
    private int[] recursive(int[] count, int index) {
        if (index == count.length - 1) {
            return new int[]{count[index], 1};
        }
        int[] nextRes = recursive(count, index+1);
        return new int[]{count[index] * nextRes[0] + nextRes[1], nextRes[0]};
    }

    public int[] fraction(int[] cont) {
        return recursive(cont, 0);
    }
}
```












