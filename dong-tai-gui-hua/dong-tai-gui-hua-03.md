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
    let sum = nums.reduce((s, n) => s + n);

    if (sum % 2)
        return false;

    let dp = new Array(sum / 2 + 1).fill(0);

    for (let i = 0; i < nums.length; ++i) {
        for (let j = sum / 2; j >= nums[i]; --j) {
            dp[j] = Math.max(dp[j], dp[j - nums[i]] + nums[i]);
        }
    }

    return dp[sum / 2] === sum / 2;
};
```

## **最后一块石头的重量 II**

和上题类似，不过返回值不同。

```javascript
/**
 * @param {number[]} stones
 * @return {number}
 */
var lastStoneWeightII = function (stones) {
    let sum = stones.reduce((s, n) => s + n);

    let dpLen = Math.floor(sum / 2);
    let dp = new Array(dpLen + 1).fill(0);

    for (let i = 0; i < stones.length; ++i) {
        for (let j = dpLen; j >= stones[i]; --j) {
            dp[j] = Math.max(dp[j], dp[j - stones[i]] + stones[i]);
        }
    }

    return sum - dp[dpLen] - dp[dpLen];
};
```



