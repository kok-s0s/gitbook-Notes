# 贪心算法-05

## 单调递增的数字

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var monotoneIncreasingDigits = function (n) {
    let nStr = n.toString().split("").map(num => +num);
    let changeIndex = nStr.length;

    for (let i = nStr.length - 1; i > 0; --i) {
        if (nStr[i - 1] > nStr[i]) {
            changeIndex = i;
            nStr[i - 1]--;
        }
    }

    for (let i = changeIndex; i < nStr.length; ++i) {
        nStr[i] = 9;
    }

    return +nStr.join("");
};
```

## **买卖股票的最佳时机含手续费**

**要注意何时做买卖**

做收获利润操作的时候有三种情况：

* 情况一：收获利润的这一天并不是收获利润区间里的最后一天（不是真正的卖出，相当于持有股票），所以后面要继续收获利润。
* 情况二：前一天是收获利润区间里的最后一天（相当于真正的卖出了），今天要重新记录最小价格了。
* 情况三：不作操作，保持原有状态（买入，卖出，不买不卖）

```javascript
/**
 * @param {number[]} prices
 * @param {number} fee
 * @return {number}
 */
var maxProfit = function (prices, fee) {
    let res = 0;
    let minPrices = prices[0];

    for (let i = 1; i < prices.length; ++i) {
        // 情况二：相当于买入
        if (minPrices > prices[i])
            minPrices = prices[i];

        // 情况三：保持原有状态（因为此时买则不便宜，卖则亏本）
        if (minPrices >= prices[i] - fee && minPrices <= prices[i])
            continue;

        // 计算利润，可能有多次计算利润，最后一次计算利润才是真正意义的卖出
        if (minPrices < prices[i] - fee) {
            res += (prices[i] - fee - minPrices);
            minPrices = prices[i] - fee
        }
    }

    return res;
};
```

