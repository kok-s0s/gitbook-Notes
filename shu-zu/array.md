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

