# String

## 反转字符串

### 库函数

```javascript
/**
 * @param {character[]} s
 * @return {void} Do not return anything, modify s in-place instead.
 */
var reverseString = function(s) {
    return s.reverse()
};
```

### 双指针交换

```javascript
/**
 * @param {character[]} s
 * @return {void} Do not return anything, modify s in-place instead.
 */
var reverseString = function (s) {
    let left = 0;
    let right = s.length - 1;
    while (left < right) {
        let temp = s[left];
        s[left] = s[right];
        s[right] = temp;
        left++;
        right--;
    }
    return s;
};
```

```javascript
/**
 * @param {character[]} s
 * @return {void} Do not return anything, modify s in-place instead.
 */
var reverseString = function (s) {
    for (let left = 0, right = s.length - 1; left < right; left++, right--) {
        let temp = s[left];
        s[left] = s[right];
        s[right] = temp;
    }
    return s;
};
```

## **反转字符串 II**

```javascript
/**
 * @param {string} s
 * @param {number} k
 * @return {string}
 */
var reverseStr = function (s, k) {
    let strArray = s.split('');
    for (let i = 0; i < s.length; i += 2 * k) {
        for (let left = i, right = i + k > s.length - 1 ? s.length : i + k - 1; left < right; ++left, --right)
            [strArray[left], strArray[right]] = [strArray[right], strArray[left]];
    }
    return strArray.join('');
};
```

## 替换空格

```javascript
/**
 * @param {string} s
 * @return {string}
 */
var replaceSpace = function(s) {
    return s.split(" ").join("%20");
};
```

## **翻转字符串里的单词**

```javascript
/**
 * @param {string} s
 * @return {string}
 */
var reverseWords = function (s) {
    let wsRegex = /^\s+|\s+$/g;
    return s.replace(wsRegex, "").split(/\s+/).reverse().join(" ");
};
```

##  **左旋转字符串**

```javascript
/**
 * @param {string} s
 * @param {number} n
 * @return {string}
 */
var reverseLeftWords = function (s, n) {
    const reverse = (str, left, right) => {
        let strArr = str.split("");
        for (; left < right; left++, right--) {
            [strArr[left], strArr[right]] = [strArr[right], strArr[left]];
        }
        return strArr.join("");
    }
    s = reverse(s, 0, n - 1);
    s = reverse(s, n, s.length - 1);
    return reverse(s, 0, s.length - 1);
};
```

## 实现 strStr\(\)

> KMP的经典思想:**当出现字符串不匹配时，可以记录一部分之前已经匹配的文本内容，利用这些信息避免从头再去做匹配。**

### 前缀表统一减一

```javascript
/**
 * @param {string} haystack
 * @param {string} needle
 * @return {number}
 */
var strStr = function (haystack, needle) {
    if (needle.length === 0)
        return 0;

    const getNext = (needle) => {
        let next = [];
        let j = -1;
        next.push(j);

        for (let i = 1; i < needle.length; ++i) {
            while (j >= 0 && needle[i] !== needle[j + 1])
                j = next[j];
            if (needle[i] === needle[j + 1])
                j++;
            next.push(j);
        }

        return next;
    }

    let next = getNext(needle);
    let j = -1;
    for (let i = 0; i < haystack.length; ++i) {
        while (j >= 0 && haystack[i] !== needle[j + 1])
            j = next[j];
        if (haystack[i] === needle[j + 1])
            j++;
        if (j === needle.length - 1)
            return (i - needle.length + 1);
    }

    return -1;
};
```

### 前缀表统一不减一

```javascript
/**
 * @param {string} haystack
 * @param {string} needle
 * @return {number}
 */
var strStr = function (haystack, needle) {
    if (needle.length === 0)
        return 0;

    const getNext = (needle) => {
        let next = [];
        let j = 0;
        next.push(j);

        for (let i = 1; i < needle.length; ++i) {
            while (j > 0 && needle[i] !== needle[j])
                j = next[j - 1];
            if (needle[i] === needle[j])
                j++;
            next.push(j);
        }

        return next;
    }

    let next = getNext(needle);
    let j = 0;
    for (let i = 0; i < haystack.length; ++i) {
        while (j > 0 && haystack[i] !== needle[j])
            j = next[j - 1];
        if (haystack[i] === needle[j])
            j++;
        if (j === needle.length)
            return (i - needle.length + 1);
    }

    return -1;
};
```

## 重复的子字符串

计算出最长相同前后缀的长度，用目标字符串长度减去这个长度得到的剩余长度，如果next数组的长度能整除该剩余长度，则目标字符串能由多个重复子字符串构成。

### 前缀表统一减一

```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
var repeatedSubstringPattern = function (s) {
    if (s.length === 0)
        return false;

    const getNext = (s) => {
        let next = [];
        let j = -1;

        next.push(j);

        for (let i = 1; i < s.length; ++i) {
            while (j >= 0 && s[i] !== s[j + 1])
                j = next[j];
            if (s[i] === s[j + 1])
                j++;
            next.push(j);
        }

        return next;
    }

    let next = getNext(s);

    if (next[next.length - 1] !== -1 && s.length % (s.length - (next[next.length - 1] + 1)) === 0)
        return true;
    return false;
};
```

### 前缀表统一不减一

```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
var repeatedSubstringPattern = function (s) {
    if (s.length === 0)
        return false;

    const getNext = (s) => {
        let next = [];
        let j = 0;

        next.push(j);

        for (let i = 1; i < s.length; ++i) {
            while (j > 0 && s[i] !== s[j])
                j = next[j - 1];
            if (s[i] === s[j])
                j++;
            next.push(j);
        }

        return next;
    }

    let next = getNext(s);

    if (next[next.length - 1] !== 0 && s.length % (s.length - next[next.length - 1]) === 0)
        return true;
    return false;
};
```



