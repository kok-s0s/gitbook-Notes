# 贪心算法-01

## 分发饼干

```javascript
/**
 * @param {number[]} g
 * @param {number[]} s
 * @return {number}
 */
var findContentChildren = function (g, s) {
    g.sort((a, b) => a - b);
    s.sort((a, b) => a - b);
    let index = 0;
    let res = 0;
    for (let i = 0; i < s.length; ++i) {
        if (s[i] >= g[index]) {
            res++;
            index++;
        }
    }
    return res;
};
```

## 摆动序列

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var wiggleMaxLength = function (nums) {
    let len = nums.length;
    if (len < 2)
        return len;

    let preDiff = nums[1] - nums[0];
    let res = preDiff === 0 ? 1 : 2;

    for (let i = 2; i <= nums.length; ++i) {
        let diff = nums[i] - nums[i - 1];
        if ((preDiff >= 0 && diff < 0) || (preDiff <= 0 && diff > 0)) {
            res++;
            preDiff = diff;
        }
    }

    return res;
};
```

## 最大子序和

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function (nums) {
    let count = 0;
    let res = Number.MIN_SAFE_INTEGER;

    for (let i = 0; i < nums.length; ++i) {
        count += nums[i];
        res = count > res ? count : res;
        count = count < 0 ? 0 : count;
    }

    return res;
};
```

