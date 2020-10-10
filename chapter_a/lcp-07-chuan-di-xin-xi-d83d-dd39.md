# LCP 07 传递信息🔹

```text
小朋友 A 在和 ta 的小伙伴们玩传信息游戏，游戏规则如下： 
* 有 n 名玩家，所有玩家编号分别为 0 ～ n-1，其中小朋友 A 的编号为 0 每个玩家都有固定的若干个可传信息的其他玩家（也可能没有）。传信息的关系是单向的（比如 A 可以向 B 传信息，但 B 不能向 A 传信息）。 
* 每轮信息必须需要传递给另一个人，且信息可重复经过同一个人。 
给定总玩家数 n，以及按 [玩家编号,对应可传递玩家编号] 关系组成的二维数组 relation。返回信息从小 A (编号 0 ) 经过 k 轮传递到编号为 n-1 的小伙伴处的方案数；若不能到达，返回 0。 

示例 1： 
输入：n = 5, relation = [[0,2],[2,1],[3,4],[2,3],[1,4],[2,0],[0,4]], k = 3 
输出：3 
解释：信息从小 A 编号 0 处开始，经 3 轮传递，到达编号 4。共有 3 种方案，分别是 0->2->0->4， 0->2->1->4， 0->2->3->4。 

示例 2： 
输入：n = 3, relation = [[0,2],[2,1]], k = 2 
输出：0 
解释：信息不能从小 A 处经过 2 轮传递到编号 2 
限制： 
2 <= n <= 10 
1 <= k <= 5 
1 <= relation.length <= 90, 且 relation[i].length == 2 
0 <= relation[i][0],relation[i][1] < n 且 relation[i][0] != relation[i][1]
```

## 方法一、深度优先搜索

使用 `Map<Integer, List<Integer>>` 存储 relation 中的对应关系。

```java
class Solution {
    private int count;
    public int numWays(int n, int[][] relation, int k) {
        Map<Integer, List<Integer>> map = new HashMap<>();
        for(int [] temp : relation){
            if(!map.containsKey(temp[0]))
                map.put(temp[0], new ArrayList<>());
            map.get(temp[0]).add(temp[1]);
        }
        count = 0;
        backTracking(map,k,n,0,0);
        return count;
    }

    public void backTracking(Map<Integer, List<Integer>> map, int k, int n, int cur, int curPerson) {
        if(cur==k){
            if(curPerson==n-1) count++;
            return ;
        }
        if(!map.containsKey(curPerson)) return;
        for(int i: map.get(curPerson)){
            backTracking(map,k,n,cur+1,i);
        }
    }
}
```

**复杂度分析**

* 时间复杂度：O\(n^k\)。
* 空间复杂度：O\(n\)。

## 方法二、动态规划

思路： 1 `dp[i][j]` 表示数组的第 `i` 轮传递给编号 `j` 的人的方案数。

2 若能传递给编号 y 玩家的所有玩家编号 x1,x2,x3... , 则第 i+1 轮传递信息给编号 y 玩家的递推方程为： `dp[i+1][y] = sum(dp[i][x1],dp[i][x2],dp[i][x3]...)`

3 递推形式即 `dp[i+1][y] += dp[i][x]`，x 与 y 为一组 relation 对应组。

```java
class Solution {
    private int count;
    public int numWays(int n, int[][] relation, int k) {
        int[][] dp = new int[k+1][n+1];
        dp[0][0]=1;

        for(int i=0;i<k;i++){
            for(int[] temp : relation){
                dp[i+1][temp[1]] += dp[i][temp[0]];
            }
        }
        return dp[k][n-1];
    }
}
```

**复杂度分析**

* 时间复杂度：O\(k\*n^2\)。
* 空间复杂度：O\(k∗n\)。

