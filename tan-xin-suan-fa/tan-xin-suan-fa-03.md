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



