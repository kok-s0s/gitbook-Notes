# 动态规划-03

##  分割等和子集

01背包问题，要明确每个元素只能够去一次，且背包容量要明确是所有数字总和的一半。

> **dp\[i\]表示 背包总容量是i，最大可以凑成i的子集总和为dp\[i\]**。



```javascript
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var canPartition = function (nums) {
    let sum = 0;

    for (let num of nums) {
        sum += num;
    }

    if (sum % 2)
        return false;

    let dpLen = Math.floor(sum / 2);
    let dp = new Array(dpLen + 1).fill(0);

    for (let i = 0; i < nums.length; ++i) {
        for (let j = dpLen; j >= nums[i]; --j) {
            dp[j] = Math.max(dp[j], dp[j - nums[i]] + nums[i]);
        }
    }

    return dp[dpLen] === dpLen;
};
```

