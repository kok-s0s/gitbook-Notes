# 回溯算法-03

## **子集 II**

```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var subsetsWithDup = function (nums) {
    let res = [];
    let path = [];

    const backTracking = (startIndex) => {
        res.push([...path]);
        if (startIndex >= nums.length)
            return;

        for (let i = startIndex; i < nums.length; ++i) {
            if (i > startIndex && nums[i - 1] === nums[i])
                continue;

            path.push(nums[i]);
            backTracking(i + 1);
            path.pop();
        }
    }

    nums.sort((a, b) => a - b);
    backTracking(0);

    return Array.from(new Set(res));
};
```

## 递增子序列

```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var findSubsequences = function (nums) {
    let res = [];
    let path = [];

    const backTracking = (startIndex) => {
        if (path.length >= 2)
            res.push([...path]);

        let used = new Set();
        for (let i = startIndex; i < nums.length; ++i) {
            if (used.has(nums[i]) || (nums[i] < path[path.length - 1] && path.length > 0))
                continue;

            used.add(nums[i]);
            path.push(nums[i]);
            backTracking(i + 1);
            path.pop();
        }
    }

    backTracking(0);
    return res;
};
```

## 全排列

```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var permute = function (nums) {
    let res = [];
    let path = [];
    let used = new Array(nums.length).fill(false);

    const backTracking = () => {
        if (path.length === nums.length) {
            res.push([...path]);
            return
        }

        for (let i = 0; i < nums.length; ++i) {
            if (!used[i]) {
                path.push(nums[i]);
                used[i] = true;
                backTracking();
                path.pop();
                used[i] = false
            }
        }
    }

    backTracking(0);

    return res;
};
```

## 全排列 II

```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var permuteUnique = function (nums) {
    let res = [];
    let path = [];
    let used = new Array(nums.length).fill(false);

    const backTracking = () => {
        if (path.length === nums.length) {
            res.push([...path]);
            return;
        }

        for (let i = 0; i < nums.length; ++i) {
            if ((i > 0 && nums[i - 1] === nums[i]) && !used[i - 1])
                continue;
            if (!used[i]) {
                path.push(nums[i]);
                used[i] = true;
                backTracking();
                used[i] = false;
                path.pop();
            }
        }
    }

    nums.sort((a, b) => a - b);
    backTracking();

    return res;
};
```



