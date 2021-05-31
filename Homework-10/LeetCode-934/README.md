# LeetCode-934

题目链接：https://leetcode.com/problems/shortest-bridge/

## 算法思路

给定一张0-1标记的地图，连通的1是岛屿，0是海洋。其中，地图上一定只有两个岛屿。现要求最少将多少个海洋变成陆地，可以将两个岛屿合并为一个。

这和染色问题类似。首先，为了区分两个岛屿，对地图进行遍历，当找到任何一个岛屿的陆地块(1标记)时，开始dfs，对该岛屿所有陆地进行染色（陆地标记转变成2，从而与另一个岛屿陆地标记1区分）。接着，对2号岛屿进行扩张染色。即，每次遍历地图，针对指定颜色，将其附近的海洋块染色为（指定颜色数值+1）。在该过程中，如果遇到1号标记岛屿，则停止染色，找到答案。

## 代码

```cpp
class Solution {
private:
    int r = 0;
    int c = 0;
public:
    int color(vector<vector<int>> &g, int x, int y) {
        // boundary
        if (x < 0 or x >= r or y < 0 or y >= c or g[x][y] != 1)
            return 0;

        // color island
        g[x][y] = 2;

        // dfs color
        return color(g, x+1, y) + color(g, x-1, y) +
            color(g, x, y+1) + color(g, x, y-1) + 1;
    }

    bool expand(vector<vector<int>> &g, int x, int y, int color) {
        // boundary
        if (x < 0 or x >= r or y < 0 or y >= c)
            return false;

        // expand
        if (g[x][y] == 0)   g[x][y] = color + 1;

        // check if reach other island
        return g[x][y] == 1;
    }

    int shortestBridge(vector<vector<int>>& g) {
        r = g.size(), c = g[0].size();

        // color one island to seperate two island
        for (int i = 0, found = 0; !found and i < r; ++i)
            for (int j = 0; !found and j < c; ++j)
                found = color(g, i, j);

        for (int color = 2; ; ++color)
            for (int x = 0; x < r; ++x)
                for (int y = 0; y < c; ++y)
                    if (g[x][y] == color and (expand(g, x+1, y, color) or
                        expand(g, x-1, y, color) or expand(g, x, y+1, color) or
                        expand(g, x, y-1, color)))
                        return color - 2;
    }
};
```

## 测试截图

![img](./accept.png)
