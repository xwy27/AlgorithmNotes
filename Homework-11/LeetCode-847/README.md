# LeetCode-847

题目链接：https://leetcode.com/problems/shortest-path-visiting-all-nodes/

## 算法思路

给定一个无向图，其中边权均为1。题目要求，从图中任意一个节点开始，可多次经过同一条边以及节点，最终遍历完图中所有节点所需要经过的最短路径数目。

可以使用动态规划来完成题目。首先定义 dis[i][j] 表示图中任意两个节点之间的最短路径，给定图之后，使用 floyd 算法可以获得 dis[i][j] 的所有值。

接着定义动态规划数组：dp[i][j]，它表示，对于节点组i，遍历完所有节点后，最终停在节点j时，所需要的最短路径数目。其中，节点组i为二进制数对应的十进制数，而相应的二进制数属于bit mask，标志着节点组中含有哪些节点。如，节点组（1，4，5）表示为（11001），即i为25。动态规划的状态转移方程为：dp[i+v][j] = min(dp[i+v][j], dp[i][j]+dis[v][j])，即对于一个不包含节点v的节点组，加入节点v后，遍历所有组中节点后，停留在j的最短路径数目为原始值，和加入其中后加上从v到j的最短路径数目，二者的最小值。

## 代码

```cpp
class Solution {
private:

public:
    int shortestPathLength(vector<vector<int>>& graph) {
        // distance matrix
        vector<vector<int>> dis(15, vector<int>(15, 0x3f));

        // dp[i][j] => min paths to travel nodes group and stop at node j
        // nodes group is represented in binary form
        // e.g. nodes(1, 4, 5) -> 11001 -> 25, where i is 25
        vector<vector<int>> dp(1<<13, vector<int>(13, 0x3f));

        int n = graph.size();
        for (int node = 0; node < n; ++node)
            for (auto &nei : graph[node])
                dis[node][nei] = 1;

        // floyd algorithm
        for (int k = 0; k < n; ++k)
            for (int i = 0; i < n; ++i)
                for (int j = 0; j < n; ++j)
                    dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j]);

        // init dp array
        for (int i = 0; i < n; ++i)
            dp[1<<i][i] = 0; // only travel node i

        // dp[i][j] = min(dp[i][j], dp[i+v][j]+dis[v][j])
        // Iterate node(i) that is in the "group" and pick a node(j) that is not in the "group"
        // Then update dp[group+j][j] by minimum value between dp[group+j][j] and dp[group][i] + dis[i][v]
        for (int group = 1; group < (1<<n); ++group)
            for (int i = 0; i < n; ++i)
                for (int j = 0; j < n; ++j) {
                    int src = 1 << i, dst = 1 << j;
                    // src in the node group and dst not
                    if ((group & src) and !(group & dst))
                        dp[group|dst][j] = min(dp[group|dst][j], dp[group][i] + dis[i][j]);
                }


        int ans = INT_MAX;
        for (int i = 0; i < n; ++i)
            ans = min(ans, dp[(1<<n)-1][i]);

        return ans;
    }
};
```

## 测试截图

![img](./accept.png)
