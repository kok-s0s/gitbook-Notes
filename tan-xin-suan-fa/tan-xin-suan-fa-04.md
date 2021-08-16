# 贪心算法-04

## 用最少的箭引爆气球

按照每个气球的左边起始位置做升序排序，在不断更新最小的右边边界，做边界判断以此推断是否需要更多的箭。

```javascript
/**
 * @param {number[][]} points
 * @return {number}
 */
var findMinArrowShots = function (points) {
    if (points.length === 0)
        return 0;

    points.sort((a, b) => {
        return a[0] - b[0];
    })

    let arrow = 1;

    for (let i = 1; i < points.length; ++i) {
        if (points[i][0] > points[i - 1][1])
            arrow++;
        else {
            points[i][1] = Math.min(points[i][1], points[i - 1][1]);
        }
    }

    return arrow;
};
```

## 无重叠区间

题目要求和上题类似，按照每个区间的右边界来做升序排序，从头开始遍历，找出有多少个区间无任何重叠，最后用总区间个数减去无重叠个数得到该删去区间个数。

```javascript
/**
 * @param {number[][]} intervals
 * @return {number}
 */
var eraseOverlapIntervals = function (intervals) {
    if (!intervals.length)
        return 0;

    intervals.sort((a, b) => {
        return a[1] - b[1];
    });

    let noOverlap = 1;
    let minRight = intervals[0][1];

    for (let i = 1; i < intervals.length; ++i) {
        if (intervals[i][0] >= minRight) {
            noOverlap++;
            minRight = intervals[i][1];
        }
    }

    return intervals.length - noOverlap;
};
```

## 划分字母区间

```javascript
/**
 * @param {string} s
 * @return {number[]}
 */
var partitionLabels = function (s) {
    let hashTable = new Array(27).fill(-1);
    let res = [];
    for (let i = 0; i < s.length; ++i) {
        hashTable[s[i].charCodeAt() - 'a'.charCodeAt()] = i;
    }

    let left = 0;
    let right = 0;

    for (let i = 0; i < s.length; ++i) {
        right = Math.max(right, hashTable[s[i].charCodeAt() - 'a'.charCodeAt()]);
        if (i === right) {
            res.push(right - left + 1);
            left = i + 1;
        }
    }

    return res;
};
```

## 合并区间

```javascript
/**
 * @param {number[][]} intervals
 * @return {number[][]}
 */
var merge = function (intervals) {
    if (!intervals.length)
        return [];

    let mergedRes = [];

    intervals.sort((a, b) => {
        return a[0] - b[0];
    });

    let left, right;
    mergedRes.push(intervals[0])

    for (let i = 1; i < intervals.length; ++i) {
        left = intervals[i][0];
        right = intervals[i][1];

        if (left > mergedRes[mergedRes.length - 1][1])
            mergedRes.push(intervals[i]);
        else
            mergedRes[mergedRes.length - 1][1] = Math.max(mergedRes[mergedRes.length - 1][1], right);
    }

    return mergedRes;
};
```

