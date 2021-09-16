# 动态规划-04

## 目标和

根据题意，target 已知，所有数总和 sum 能够计算出来，可以将整个数组分为两大部分（左右两部分，left 和 right ），则存在关系 left + right = sum；left - right = traget；能推导出 left - \( sum - left \) = traget，则有 2 \* left = target + sum；问题能够转化装满容量为 left 的背包一共会有多少种方法。

> 在求装满背包有几种方法的情况下，递推公式一般为：
>
> ```text
> dp[j] += dp[j - nums[i]];
> ```

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var findTargetSumWays = function (nums, target) {
    let sum = nums.reduce((s, n) => s + n);

    if (sum < Math.abs(target) || (sum + target) % 2 === 1)
        return 0;

    let left = Math.floor((sum + target) / 2);

    let dp = new Array(left + 1).fill(0);
    dp[0] = 1;

    for (let i = 0; i < nums.length; ++i) {
        for (let j = left; j >= nums[i]; --j) {
            dp[j] += dp[j - nums[i]];
        }
    }

    return dp[left];
};
```

## 一和零

有两个判断纬度的01背包问题

```javascript
/**
 * @param {string[]} strs
 * @param {number} m
 * @param {number} n
 * @return {number}
 */
var findMaxForm = function (strs, m, n) {
    let dp = new Array(m + 1).fill().map(() => Array(n + 1).fill(0));

    let numOfZeros, numOfOnes;

    for (let item of strs) {
        numOfZeros = 0;
        numOfOnes = 0;

        for (let c of item) {
            if (c === "0")
                numOfZeros++;
            else
                numOfOnes++;
        }

        for (let i = m; i >= numOfZeros; --i) {
            for (let j = n; j >= numOfOnes; --j) {
                dp[i][j] = Math.max(dp[i][j], dp[i - numOfZeros][j - numOfOnes] + 1);
            }
        }
    }

    return dp[m][n];
};
```

