# LeetCode-56

题目链接：https://leetcode.com/problems/merge-intervals/

## 算法思路

题目要求合并一个区间的数组，使得数组内不存在相交的区间。解决的思路很简单，首先对原区间数组进行排序，排序规则为：区间起点不同时，按照区间起点降序排列，相同时则按照区间终点降序排列（直接sort即可，这个就是默认规则）。我们维护一个区间起点和终点变量，并在遍历排序后的区间数组时，对维护的区间进行更新。

由于区间已经降序排列，所以只会出现两种情况：下一个区间与当前区间无交集，将当前区间加入答案数组；下一个区间与当前区间有交集，更新最大的终点为区间终点即可（起点必定是当前维护的区间起点，因为数组降序，且从头遍历）。

时间复杂度为$O(nlogn)$，排序时间复杂度。


## 代码

```cpp
class Solution {
public:    
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int>> ans;
        if (intervals.size() < 1)  return ans;

        sort(intervals.begin(), intervals.end());

        int start = intervals[0][0], end = intervals[0][1];
        for (int i = 1; i < intervals.size(); ++i) {
            if (intervals[i][0] > end) {
                ans.push_back({start, end});
                start = intervals[i][0];
                end = intervals[i][1];
            } else {
                end = max(intervals[i][1], end);
            }
        }
        
        ans.push_back({start, end});
        return ans;
    }
};
```

## 测试截图

![img](./accept.png)
