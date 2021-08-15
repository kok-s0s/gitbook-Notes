# 贪心算法-03

## 加油站

### 暴力解法

双层循环遍历每一个节点的遍历情况

> **for循环适合模拟从头到尾的遍历，而while循环适合模拟环形遍历，要善于使用while！**

```javascript
/**
 * @param {number[]} gas
 * @param {number[]} cost
 * @return {number}
 */
var canCompleteCircuit = function (gas, cost) {
    for (let i = 0; i < cost.length; ++i) {
        let rest = gas[i] - cost[i];
        let curIndex = (i + 1) % cost.length;

        while (rest > 0 && curIndex !== i) {
            rest += gas[curIndex] - cost[curIndex];
            curIndex = (curIndex + 1) % cost.length;
        }

        if (rest >= 0 && curIndex === i)
            return i;
    }
    return -1;
};
```

### 贪心法-001

直接从全局进行贪心选择，情况如下：

* 情况一：如果gas的总和小于cost总和，那么无论从哪里出发，一定是跑不了一圈的
* 情况二：rest\[i\] = gas\[i\]-cost\[i\]为一天剩下的油，i从0开始计算累加到最后一站，如果累加没有出现负数，说明从0出发，油就没有断过，那么0就是起点。
* 情况三：如果累加的最小值是负数，汽车就要从非0节点出发，从后向前，看哪个节点能这个负数填平，能把这个负数填平的节点就是出发节点。

```javascript
/**
 * @param {number[]} gas
 * @param {number[]} cost
 * @return {number}
 */
var canCompleteCircuit = function (gas, cost) {
    let min = Number.MAX_SAFE_INTEGER;
    let rest = 0;
    let restSum = 0;

    for (let i = 0; i < cost.length; ++i) {
        rest = gas[i] - cost[i];
        restSum += rest;
        min = Math.min(min, restSum);
    }

    if (restSum < 0)
        return -1;
    if (min >= 0)
        return 0;
    for (let i = cost.length - 1; i >= 0; --i) {
        min += gas[i] - cost[i];
        if (min >= 0)
            return i;
    }
    return -1;
};
```

### 贪心法-002

**局部最优：当前累加rest\[j\]的和curSum一旦小于0，起始位置至少要是j+1，因为从j开始一定不行。全局最优：找到可以跑一圈的起始位置**。

```javascript
/**
 * @param {number[]} gas
 * @param {number[]} cost
 * @return {number}
 */
var canCompleteCircuit = function (gas, cost) {
    let curSum = 0;
    let totalSum = 0;
    let rest = 0;
    let start = 0;

    for (let i = 0; i < cost.length; ++i) {
        rest = gas[i] - cost[i];
        curSum += rest;
        totalSum += rest;

        if (curSum < 0) {
            start = i + 1;
            curSum = 0;
        }
    }
    if (totalSum < 0)
        return -1;
    return start;
};
```



## 分发糖果

采用了两次贪心的策略：

* 一次是从左到右遍历，只比较右边孩子评分比左边大的情况。
* 一次是从右到左遍历，只比较左边孩子评分比右边大的情况。

这样从局部最优推出了全局最优，即：相邻的孩子中，评分高的孩子获得更多的糖果。

```javascript
/**
 * @param {number[]} ratings
 * @return {number}
 */
var candy = function (ratings) {
    let candyArr = new Array(ratings.length).fill(1);

    for (let i = 1; i < ratings.length; ++i) {
        if (ratings[i] > ratings[i - 1])
            candyArr[i] = candyArr[i - 1] + 1;
    }

    for (let i = ratings.length - 2; i >= 0; --i) {
        if (ratings[i] > ratings[i + 1])
            candyArr[i] = Math.max(candyArr[i], candyArr[i + 1] + 1);
    }

    return candyArr.reduce((totalCandys, temp) => {
        return totalCandys + temp;
    })
};
```

## 柠檬水找零

```javascript
/**
 * @param {number[]} bills
 * @return {boolean}
 */
var lemonadeChange = function (bills) {
    let restFive = 0;
    let restTen = 0;

    for (let i = 0; i < bills.length; ++i) {
        if (bills[i] === 5)
            restFive++;
        else if (bills[i] === 10) {
            if (restFive) {
                restFive--;
                restTen++;
            } else
                return false;
        } else if (bills[i] === 20) {
            if (restTen && restFive) {
                restTen--;
                restFive--;
            } else if (restFive >= 3) {
                restFive -= 3;
            } else
                return false;
        }
    }

    return true;
};
```

## 根据身高重建队列

先根据队列中的身高按大到小排列，再维护一个新的序列，依次从排序的序列取队首插入到新的序列中。

```javascript
/**
 * @param {number[][]} people
 * @return {number[][]}
 */
var reconstructQueue = function (people) {
    people.sort((a, b) => {
        if (a[0] !== b[0])
            return b[0] - a[0];
        else
            return a[1] - b[1];
    });

    let queue = [];

    for (let i = 0; i < people.length; ++i)
        queue.splice(people[i][1], 0, people[i]);

    return queue;
};
```



