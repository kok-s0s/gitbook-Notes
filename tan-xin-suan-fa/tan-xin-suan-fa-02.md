# 贪心算法-02

## **买卖股票的最佳时机 II**

**将利润分解为每天来进行计算**

```javascript
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function (prices) {
    let maxProfit = 0;
    
    for (let i = 1; i <= prices.length; ++i) {
        if (prices[i] > prices[i - 1])
            maxProfit += (prices[i] - prices[i - 1]);
    }

    return maxProfit;
};
```

## 跳跃游戏

**跳跃覆盖范围究竟是否可以覆盖到终点**

```javascript
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var canJump = function(nums) {
    let maxRight = 0;

    for(let i = 0; i < nums.length; ++i) {
        if(i > maxRight)
            return false;
        maxRight = Math.max(maxRight, i + nums[i]);
    }

    return true;
};
```

## 跳跃游戏 II

就是原本跳跃游戏的“接头版本“

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var jump = function (nums) {
    let curDistance = 0;
    let nexDistance = 0;
    let step = 0;

    for (let i = 0; i < nums.length - 1; ++i) {
        nexDistance = Math.max(nexDistance, i + nums[i])
        if (i === curDistance) {
            curDistance = nexDistance;
            step++;
        }
    }

    return step;
};
```

## K次取反后最大化的数组和

两次贪心

```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var largestSumAfterKNegations = function (nums, k) {
    nums.sort((a, b) => Math.abs(b) - Math.abs(a));

    for (let i = 0; i < nums.length; ++i) {
        if (nums[i] < 0 && k > 0) {
            nums[i] *= -1;
            k--;
        }
    }

    if (k > 0 && k % 2 === 1)
        nums[nums.length - 1] *= -1;

    return nums.reduce((a, b) => {
        return a + b;
    });
};
```



