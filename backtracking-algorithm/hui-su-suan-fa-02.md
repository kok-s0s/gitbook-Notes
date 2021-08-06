# 回溯算法-02

## 组合总和

### 无剪枝

```javascript
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */
var combinationSum = function (candidates, target) {
    let res = [];
    let path = [];

    const backTracking = (startIndex, sum) => {
        if (sum > target)
            return;

        if (sum === target) {
            res.push([...path]);
            return;
        }

        for (let i = startIndex; i < candidates.length; ++i) {
            path.push(candidates[i]);
            backTracking(i, sum + candidates[i]);
            path.pop();
        }
    }

    backTracking(0, 0)

    return res;
};
```

### 剪枝

```javascript
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */
var combinationSum = function (candidates, target) {
    let res = [];
    let path = [];

    const backTracking = (startIndex, sum) => {
        if (sum === target) {
            res.push([...path]);
            return;
        }

        for (let i = startIndex; i < candidates.length && sum + candidates[i] <= target; ++i) {
            path.push(candidates[i]);
            backTracking(i, sum + candidates[i]);
            path.pop();
        }
    }

    candidates.sort((a, b) => a - b);
    backTracking(0, 0)

    return res;
};
```

## **组合总和 II**

### 辅助判断数组

```javascript
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */
var combinationSum2 = function (candidates, target) {
    let res = [];
    let path = [];
    let flag = new Array(candidates.length).fill(0);

    const backTracking = (startIndex, sum) => {
        if (sum === target) {
            res.push([...path]);
            return;
        }

        for (let i = startIndex; i < candidates.length && sum + candidates[i] <= target; ++i) {
            if (i > 0 && candidates[i] === candidates[i - 1] && flag[i - 1] === 0)
                continue;

            path.push(candidates[i]);
            flag[i] = 1;
            backTracking(i + 1, sum + candidates[i]);
            flag[i] = 0;
            path.pop();
        }
    }

    candidates.sort((a, b) => a - b);
    backTracking(0, 0);

    return res;
};
```

### 依靠 startIndex

```javascript
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */
var combinationSum2 = function (candidates, target) {
    let res = [];
    let path = [];

    const backTracking = (startIndex, sum) => {
        if (sum === target) {
            res.push([...path]);
            return;
        }

        for (let i = startIndex; i < candidates.length && sum + candidates[i] <= target; ++i) {
            if (i > startIndex && candidates[i] === candidates[i - 1])
                continue;

            path.push(candidates[i]);
            backTracking(i + 1, sum + candidates[i]);
            path.pop();
        }
    }

    candidates.sort((a, b) => a - b);
    backTracking(0, 0);

    return res;
};
```

## 分割回文串

```javascript
/**
 * @param {string} s
 * @return {string[][]}
 */
var partition = function (s) {
    let res = [];
    let path = [];

    const palindrome = (left, right) => {
        for (let l = left, r = right; l <= r; l++, r--) {
            if (s[l] !== s[r])
                return false;
        }
        return true;
    }

    const backTracking = (startIndex) => {
        if (startIndex >= s.length) {
            res.push([...path]);
            return;
        }

        for (let i = startIndex; i < s.length; ++i) {
            if (!palindrome(startIndex, i))
                continue;

            path.push(s.substr(startIndex, i - startIndex + 1));
            backTracking(i + 1);
            path.pop();
        }
    }

    backTracking(0);

    return res;
};
```

## 复原 IP 地址

```javascript
/**
 * @param {string} s
 * @return {string[]}
 */
var restoreIpAddresses = function (s) {
    let res = [];
    let path = [];

    const backTracking = (tempEnd) => {
        if (path.length > 4)
            return;
        if (path.length === 4 && tempEnd === s.length) {
            res.push(path.join("."));
            return;
        }

        for (let i = tempEnd; i < s.length; ++i) {
            let str = s.substr(tempEnd, i - tempEnd + 1);
            if (str.length > 3 || +str > 255 || (str.length > 1 && str[0] === '0'))
                break;

            path.push(str);
            backTracking(i + 1);
            path.pop();
        }
    }

    backTracking(0);

    return res;
};
```

## 子集

```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var subsets = function (nums) {
    let res = [];
    let path = [];

    const backTracking = (startIndex) => {
        res.push([...path]);
        if (startIndex >= nums.length) {
            return;
        }

        for (let i = startIndex; i < nums.length; ++i) {
            path.push(nums[i]);
            backTracking(i + 1);
            path.pop();
        }
    }

    backTracking(0);

    return res;
};
```

