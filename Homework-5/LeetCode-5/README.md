# LeetCode-5

题目链接：https://leetcode.com/problems/longest-palindromic-substring/

## 算法思路

题目要求给定字符串中的最长回文串。如果遍历字符串的所有字串(O(n^2))，再判断是否是回文串$O(n)$，则需要O(n^3)的时间复杂度。可以考虑利用已经判断过的回文串结果，对之后判断的回文串进行辅助，如：已知子串 s[i+1, ..., j-1]是回文串，那判断 s[i, ..., j]只需要判断 s[i] 是否等于 s[j]，那么回文串的判断就变为了O(1)复杂度。

前面的分析已经给出了一个转移方程，所以考虑动态规划解决这个问题。定义状态dp[i][j]，表示子串s[i, ..., j] 是否是回文串，转移方程则为：

dp[i][j] = s[i] == s[j] and (j - i < 3 or dp[i+1][j-1])

因为长度为2的子串，dp[i+1][j-1]不存在，所以利用 j-i < 3 提前处理。同理，长度为1的子串也在这里可以处理，所以这是一个通用公式。最终，时间复杂度为O(n^2)。

*实现细节*

1. dp数组初始化长度为1的情况，因此更新状态时，j可以从i+1开始遍历
2. 考察状态转移方程，dp[i][j] = s[i] == s[j] and (j - i < 3 or dp[i+1][j-1])，i依赖于后面的值，而j依赖于较小的值，因此写循环时，应该让i从大到小，j从小到大（动态规划里的实现小技巧，考察转移方程，确定循环方向）

## 代码

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        int size = s.size(), beg = 0, end = 0;
        vector<vector<bool>> dp(size, vector<bool>(size, false));
        for (int i = 0; i < size; ++i)
            dp[i][i] = true;
        
        for (int i = size - 1; i >= 0; --i) {
            for (int j = i + 1; j < size; ++j) {
                dp[i][j] = s[i] == s[j] and ((j < i + 3) or dp[i+1][j-1]);
                if (dp[i][j] and end - beg < j - i) {
                    beg = i, end = j;
                }
            }
        }
        
        return s.substr(beg, end-beg+1);
    }
};
```

## 测试截图

![img](./accept.png)
