# LeetCode-85

题目链接：https://leetcode.com/problems/maximal-rectangle/

## 算法思路

题目要求在给定0-1矩阵中，找到最大的全1矩阵面积。这个题目和Largest Rectangle in Histogram这道题目很类似，后者求的是直方图中最大的矩阵面积。我们可以利用这个题目的解法来求解当前问题。因为，我们可以把当前问题按行拆解，每次求解以当前行为坐标轴的直方图最大矩阵面积。

首先，我们遍历矩阵，构造出每一行为坐标轴时的直方图高度，即坐标轴上为1的位置有高度(按行往上扫描)，为0则没有。接着，遍历每一个直方图，求解最大矩阵面积，即可。
关于如何求解直方图最大矩形面积，可以采用高度的单调栈。因为递增高度的最大矩形，一定是高为最高高度的矩形。当高度非递增时，从栈中找到比高度矮的高，计算矩形面积(木桶效应：宽度递增，最矮的高限制面积)。每次计算统计最大面积即可。

## 代码

```cpp
class Solution {
public:
    int helper(vector<int> &h) {
        stack<int> s; // increasing stack
        
        h.push_back(0); // append 0 to include last element
        int sum = 0, t = 0;
        for (int i = 0; i < h.size();) {
            if (s.empty() or h[i] > h[s.top()])
                // increasing height, push current index
                s.push(i++);
            else {
                // non-increasing, retrieve element in stack
                t = s.top(), s.pop();
                // calculate current area:
                // the first non-lower height * width
                sum = max(sum, h[t] * (s.empty() ? i : i - s.top() - 1));
            }
        }
            
        return sum;
    }
    
    int maximalRectangle(vector<vector<char>>& matrix) {
        int r = matrix.size();
        if (r < 1)  return 0;
        int c = matrix[0].size();
        
        vector<vector<int>> heights(r, vector<int>(c, 0));
        for (int i = 0; i < r; ++i)
            for (int j = 0; j < c; ++j)
                if (matrix[i][j] == '0')
                    heights[i][j] = 0;
                else
                    heights[i][j] = (i == 0) ? 1 : heights[i-1][j]+1;
        
        int ans = 0;
        for (int i = 0; i < r; ++i)
            ans = max(ans, helper(heights[i]));
        
        return ans;
    }
};
```

## 测试截图

![img](./accept.png)
