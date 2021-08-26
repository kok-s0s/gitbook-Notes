# 动态规划-02

## 不同路径

```javascript
/**
 * @param {number} m
 * @param {number} n
 * @return {number}
 */
var uniquePaths = function (m, n) {
    let dp = new Array(m + 1).fill(new Array(n + 1).fill(1));

    for (let i = 2; i <= m; ++i) {
        for (let j = 2; j <= n; ++j) {
            dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
        }
    }

    return dp[m][n];
};
```

## 不同路径 II

```javascript
/**
 * @param {number[][]} obstacleGrid
 * @return {number}
 */
var uniquePathsWithObstacles = function (obstacleGrid) {
    let m = obstacleGrid.length;
    let n = obstacleGrid[0].length;

    let dp = new Array(m).fill().map(item => Array(n).fill(0));

    for (let i = 0; i < m && obstacleGrid[i][0] === 0; ++i)
        dp[i][0] = 1;

    for (let i = 0; i < n && obstacleGrid[0][i] === 0; ++i)
        dp[0][i] = 1;

    for (let i = 1; i < m; ++i) {
        for (let j = 1; j < n; ++j) {
            if (obstacleGrid[i][j] === 1)
                continue;
            dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
        }
    }

    return dp[m - 1][n - 1];
};
```

## 整数拆分

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var integerBreak = function (n) {
    let dp = new Array(n + 1).fill(0);

    dp[2] = 1;

    for (let i = 3; i <= n; ++i) {
        for (let j = 1; j <= i - 1; ++j) {
            dp[i] = Math.max(dp[i], Math.max(j * (i - j), j * dp[i - j]));
        }
    }

    return dp[n];
};
```

## 不同的二叉搜索树

拆分思想来找递推关系

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var numTrees = function (n) {
    let dp = new Array(n + 1).fill(0);

    dp[0] = 1;
    dp[1] = 1;

    for (let i = 2; i <= n; ++i) {
        for (let j = 1; j <= i; ++j) {
            dp[i] += dp[i - j] * dp[j - 1];
        }
    }

    return dp[n];
};

```

