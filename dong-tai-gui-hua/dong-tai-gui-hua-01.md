# 动态规划-01

> 动态规划中每一个状态都是由上一个状态推导出来的，贪心没有状态推导，而是从局部直接选最优的。

**动态规划解题要点**

1. 确定dp数组及下标的含义
2. 确定递推公式
3. dp数组如何初始化
4. 确定遍历顺序
5. 举例推导dp数组



## 斐波那契数

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var fib = function (n) {
    let dp = new Array(n + 1).fill(0);

    dp[0] = 0;
    dp[1] = 1;

    for (let i = 2; i <= n; ++i) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }

    return dp[n];
};
```

## 爬楼梯

> 斐波那契数的同类题

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var climbStairs = function (n) {
    let dp = new Array(n + 1).fill(0);

    dp[0] = 0;
    dp[1] = 1;
    dp[2] = 2;

    for (let i = 3; i <= n; ++i) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }

    return dp[n];
};
```

## 使用最小花费爬楼梯

```javascript
/**
 * @param {number[]} cost
 * @return {number}
 */
var minCostClimbingStairs = function (cost) {
    let dp = new Array(cost.length).fill(0);

    dp[0] = cost[0];
    dp[1] = cost[1];

    for (let i = 2; i < cost.length; ++i) {
        dp[i] = Math.min(dp[i - 1], dp[i - 2]) + cost[i];
    }

    return Math.min(dp[cost.length - 1], dp[cost.length - 2]);
};
```

