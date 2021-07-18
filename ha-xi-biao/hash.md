---
description: 牺牲了空间来换取时间
---

# Hash

## 有效的字母异位词

```javascript
/**
 * @param {string} s
 * @param {string} t
 * @return {boolean}
 */
var isAnagram = function (s, t) {
    if (s.length !== t.length)
        return false;
    const record = new Array(26).fill(0);
    for (let ch of s) {
        record[ch.charCodeAt() - "a".charCodeAt()]++;
    }
    for (let ch of t) {
        record[ch.charCodeAt() - "a".charCodeAt()]--;
    }
    return record.every(element => element === 0);
};
```

## 两个数组的交集

```javascript
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var intersection = function (nums1, nums2) {
    let temp = new Set(nums1);
    let result = new Set();
    for (let i = 0; i < nums2.length; ++i) {
        if (temp.has(nums2[i]) && !result.has(nums2[i]))
            result.add(nums2[i]);
    }
    return Array.from(result);
};
```

## 快乐数

```javascript
/**
 * @param {number} n
 * @return {boolean}
 */
var isHappy = function (n) {
    const getSum = (num) => {
        let sum = 0;
        while (num) {
            sum += Math.pow(num % 10, 2);
            num = Math.floor(num / 10);
        }
        return sum;
    }
    let sumSet = new Set();
    while (1) {
        let sum = getSum(n);
        if (sum === 1)
            return true;
        if (sumSet.has(sum))
            return false;
        else
            sumSet.add(sum);
        n = sum;
    }
};
```

## 两数之和

互补的思想 使用map这个数据结构

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function (nums, target) {
    let hash = new Map();
    for (let i = 0; i < nums.length; ++i) {
        if (hash.has(target - nums[i]))
            return [i, hash.get(target - nums[i])];
        hash.set(nums[i], i);
    }
    return [];
};
```

## **四数相加 II**

