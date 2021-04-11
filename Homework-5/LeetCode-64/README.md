# LeetCode-64

题目链接：https://leetcode.com/problems/minimum-path-sum/

## 算法思路

给定一个包含非负整数的矩阵，题目要求从矩阵左上角到右下角的路径最小值，其中，每次只能向右或者向下移动。这是标准的动态规划问题，定义dp[i][j]为走到矩阵第i行第j列时的最小路径和，那么转移方程为

dp[i][j] = m[i][j] + min(dp[i-1][j], dp[i][j-1])

对于第0行和第0列，分别进行初始化即可，循环均从小达到，因为转移方程依赖较小的状态值。

## 代码

```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int row = grid.size(), col = grid[0].size();
        if (row * col == 1) return grid[0][0];

        vector<vector<int>> dp(row, vector<int>(col, 0));
        dp[0][0] = grid[0][0];
        for (int i = 1; i < row; ++i)   dp[i][0] = dp[i-1][0] + grid[i][0];
        for (int j = 1; j < col; ++j)   dp[0][j] = dp[0][j-1] + grid[0][j];

        for (int i = 1; i < row; ++i)
            for (int j = 1; j < col; ++j)
                dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j];
        
        return dp[row-1][col-1];
    }
};
```

## 测试截图

![img](./accept.png)
