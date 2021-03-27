# LeetCode-16

题目链接：https://leetcode.com/problems/3sum-closest/

## 算法思路

题目要求查找数组中的一个三元组的和，这个和要最接近给定的目标值。这是三数和的变形题目，可以直接复用三数和的求解算法，即先对数组进行排序，然后遍历整个数组。

这对当前遍历的数字，使用双指针分别同时从数组剩余位置和数组尾查询数字，若三个数字和等于目标值，直接返回，否则，根据和与目标值的差值，移动指针。双指针移动的同时，计算和与当前答案哪个是更优解。

## 代码

```cpp
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());
        
        int ans = nums[0] + nums[1] + nums[2];
        for (int i = 0; i < nums.size() - 2; ++i) {
            int j = i + 1, k = nums.size() - 1;
            while (j < k) {
                int sum = nums[i] + nums[j] + nums[k];
                if (sum == target)
                    return target;
                else if (sum < target)
                    ++j;
                else if (sum > target)
                    --k;
                if (abs(sum - target) < abs(ans - target))
                    ans = sum;
            }
        }
        
        return ans;
    }
};
```

## 测试截图

![img](./accept.png)
