# LeetCode-152

题目链接：https://leetcode.com/problems/maximum-product-subarray/

## 算法思路

题目要求给定数组中连续子数组的最大乘积。与连续子数组最大和不同，负数只会使得和减小，并不会使和增大，但是在乘法中则不同：两个负数相乘，结果是正数，这会增大乘积。考虑到负数对于乘积的贡献，要么是增大(负乘积乘以负数)，要么是减小(正乘积乘以负数)，那么我们可以考虑维护一个二维的状态数组，dp[i][j]：表示从第0个元素到第i个元素的最大乘积与最小乘积，其中j只有0和1两个取值。状态转移方程为：

dp[i][0] = max(dp[i-1][0]*nums[i], dp[i-1][1]*nums[i], nums[i])
dp[i][1] = min(dp[i-1][1]*nums[i], dp[i-1][0]*nums[i], nums[i])

其含义为：最大乘积来自前一个的最大乘积与当前数相乘，或前一个最小乘积与当前数相乘，或当前数。最小乘积同理。这么看起来会比较绕，可以看下面的简化版本，更容易理解。

维护两个变量，mi 和 ma，存储从第0个元素到当前元素的最大乘积与最小乘积，如果当前数为负数，交换 mi 和 ma，因为负数会让大小关系变换，否则不做操作。
mi 和 ma 的更新则如下：

mi = min(mi * nums[i], nums[i])
ma = max(ma * nums[i], nums[i])

最终，返回mi和ma中最大的数即可。

## 代码

```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        if (nums.size() < 1)    return 0;
        
        // mantain two values of max product and min product for array[0, ..., i]
        int ans = nums[0], imin = nums[0], imax = nums[0];
        for (int i = 1; i < nums.size(); ++i) {
            // if number is negative, max and min must change
            if (nums[i] < 0)    swap(imax, imin);
     
            imax = max(nums[i], imax * nums[i]);
            imin = min(nums[i], imin * nums[i]);
            ans = max(ans, imax);
        }

        return ans;
    }
};
```

## 测试截图

![img](./accept.png)
