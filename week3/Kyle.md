
# 121. Best Time to Buy and Sell Stock

## Solution
```python
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """

        min_price = float("inf")
        max_profit = 0
        i = 0
        while (i < len(prices)):
            if prices[i] < min_price:
                min_price = prices[i]
            elif prices[i] - min_price > max_profit:
                max_profit = prices[i] - min_price
            i += 1
        return max_profit
```

## Key Takeway




# 122. Best Time to Buy and Sell Stock II

## Solution
```python

```

## Key Takeway




# 55. Jump Game

## Solution
```python
class Solution(object):

```

## Key Takeway
