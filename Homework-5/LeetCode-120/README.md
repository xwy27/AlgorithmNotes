# LeetCode-120

题目链接：https://leetcode.com/problems/triangle/

## 算法思路

题目要求三角形形状的数组，从顶到底的最小路径和，其中，在某一行的第i个元素，只能移动到下一行的第i个元素或第i+1个元素。依旧是一个动态规划问题，定义状态dp[i][j]为移动到第i行第j个元素时，最小的路径和，那么转移方程为：

dp[i][j] = min(dp[i-1][j], dp[i-1][j-1]) + t[i][j]

状态初始化时，针对三角形数组左侧的元素，dp[i-1][j-1]会越界，而右侧元素则会在dp[i-1][j]越界，更新状态值时注意这两个边界条件即可。

## 代码

```cpp
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int size = triangle.size();
        if (size == 1)  return triangle[0][0];
        
        vector<vector<int>> dp(triangle.begin(), triangle.end());
        dp[0][0] = triangle[0][0];
        for (int i = 1; i < size; ++i) {
            int row_size = dp[i].size();
            for (int j = 0; j < row_size; ++j)
                if (j == 0)
                    dp[i][0] = triangle[i][0] + dp[i-1][0];
                else if (j == row_size-1)
                    dp[i][j] = triangle[i][j] + dp[i-1][j-1];
                else
                    dp[i][j] = triangle[i][j] + min(dp[i-1][j-1], dp[i-1][j]);
        }

        int ans = INT_MAX;
        for (int i = 0; i < dp[size-1].size(); ++i)
            ans = min(ans, dp[size-1][i]);
        
        return ans;
    }
};
```

## 测试截图

![img](./accept.png)
