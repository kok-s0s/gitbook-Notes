---
description: 回溯是递归的副产品，只要有递归就会有回溯。
---

# 回溯算法-01

回溯的本质是穷举。

## 组合

```javascript
/**
 * @param {number} n
 * @param {number} k
 * @return {number[][]}
 */
var combine = function (n, k) {
    let res = [];
    let path = [];

    const backTracking = (startIndex) => {
        if (path.length === k) {
            res.push([...path]);
            return;
        }
        for (let i = startIndex; i <= n - (k - path.length) + 1; ++i) {
            path.push(i);
            backTracking(i + 1);
            path.pop();
        }
    }

    backTracking(1);
    return res;
};
```

## **组合总和 III**

```javascript
/**
 * @param {number} k
 * @param {number} n
 * @return {number[][]}
 */
var combinationSum3 = function (k, n) {
    let res = [];
    let path = [];

    const backTracking = (startIndex, sum) => {
        if (sum > n)
            return;
        if (path.length === k && sum === n) {
            res.push([...path]);
            return;
        }
        for (let i = startIndex; i <= 9 - (k - path.length) + 1; ++i) {
            path.push(i);
            backTracking(i + 1, sum + i);
            path.pop();
        }
    }

    backTracking(1, 0);
    return res;
};
```

## 电话号码的字母组合

```javascript
/**
 * @param {string} digits
 * @return {string[]}
 */
var letterCombinations = function (digits) {
    const letterMap = [
        "",
        "",
        "abc",
        "def",
        "ghi",
        "jkl",
        "mno",
        "pqrs",
        "tuv",
        "wxyz",
    ];
    let res = [];
    let str = [];

    const backTracking = (index) => {
        if (index === digits.length) {
            res.push(str.join(""));
            return;
        }

        let digit = digits[index].charCodeAt() - '0'.charCodeAt();
        let letter = letterMap[digit];

        for (let i = 0; i < letter.length; ++i) {
            str.push(letter[i]);
            backTracking(index + 1);
            str.pop()
        }
    }

    if (digits.length === 0)
        return [];
    else if (digits.length === 0)
        return letterMap[digits[0]].split("");

    backTracking(0, str);
    return res;
};
```

