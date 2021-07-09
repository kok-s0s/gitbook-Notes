---
description: 数组还好啦。
---

# Array

## 二分查找

### 左闭右闭

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var search = function (nums, target) {
    let left = 0;
    let right = nums.length - 1;
    while (left <= right) {
        let mid = Math.floor(left + (right - left) / 2);
        if (target === nums[mid])
            return mid;
        else if (target > nums[mid])
            left = mid + 1;
        else
            right = mid - 1;
    }
    return -1;
};
```

### 左闭右开

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var search = function (nums, target) {
    let left = 0;
    let right = nums.length;
    while (left < right) {
        let mid = Math.floor(left + (right - left) / 2);
        if(target > nums[mid]) 
            left = mid + 1;
        else if(target < nums[mid])
            right = mid;
        else
            return mid;
    }
    return -1;
};
```

## 移除元素

### 双指针法---快慢指针

```javascript
/**
 * @param {number[]} nums
 * @param {number} val
 * @return {number}
 */
var removeElement = function(nums, val) {
    let newLength = 0;
    for(let i = 0; i < nums.length; ++i)
        if(nums[i] !== val)
            nums[newLength++] = nums[i];
    return newLength;
};
```

## 有序数组的平方

### 暴力法

```javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var sortedSquares = function (nums) {
    for (let index in nums) {
        nums[index] = Math.pow(nums[index], 2)
    }
    return nums.sort((a, b) => a - b);
};
```

### 双指针法

```javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var sortedSquares = function (nums) {
    let left = 0;
    let right = nums.length - 1;
    let k = nums.length - 1;
    let newArr = new Array(nums.length);
    while (left <= right) {
        if (nums[left] * nums[left] < nums[right] * nums[right]) {
            newArr[k--] = nums[right] * nums[right];
            right--;
        }
        else {
            newArr[k--] = nums[left] * nums[left];
            left++;
        }
    }
    return newArr;
};
```

## 长度最小的子数组

### 暴力法

```javascript
/**
 * @param {number} target
 * @param {number[]} nums
 * @return {number}
 */
var minSubArrayLen = function (target, nums) {
    let result = Number.MAX_SAFE_INTEGER;
    let sum = 0;
    let subLength = 0;
    for (let i = 0; i < nums.length; ++i) {
        sum = 0;
        for (let j = i; j < nums.length; ++j) {
            sum += nums[j];
            if (sum >= target) {
                subLength = j - i + 1;
                result = result < subLength ? result : subLength;
                break;
            }
        }
    }
    return result === Number.MAX_SAFE_INTEGER ? 0 : result;
};
```

### 滑动窗口

> 双指针的一种方法

即**不断地调节子序列的起始位置和终止位置，从而得出我们要想的结果**。

```javascript
/**
 * @param {number} target
 * @param {number[]} nums
 * @return {number}
 */
var minSubArrayLen = function (target, nums) {
    let result = Number.MAX_SAFE_INTEGER;
    let sum = 0;
    let subLength = 0;
    let i = 0;
    for (let j = 0; j < nums.length; ++j) {
        sum += nums[j];
        while (sum >= target) {
            subLength = j - i + 1;
            result = result < subLength ? result : subLength;
            sum -= nums[i++];
        }
    }
    return result === Number.MAX_SAFE_INTEGER ? 0 : result;
};Ï
```

