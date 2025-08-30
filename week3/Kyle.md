
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
배열 안의 수가 정렬되어있지 않기에 투 포인터는 사용하지 못한다. 그러나 모든 것을 한 번의 순회에 이루기 위해서,
모든 가격 지점이 '사는 시점'이거나 '파는 시점' 둘 중 하나로 나뉜다는 것을 이해하고 두 의사결정을 한 번에 할 수 있음을 파악해야 한다.
min_price의 초기값을 부동소수점 최대값으로 잡아 언제나 첫 가격이 min_price 즉 사는 가격이 되도록 처리한다.
이후에는 순회를 돌면서 그 가격을 새로운 '사는 가격'으로 삼을지 아니면 새로운 '팔 가격'으로 삼을지만 결정하면 된다.



# 122. Best Time to Buy and Sell Stock II

## Solution
```python
class Solution(object):
    def maxProfit(self, prices):
        profit = 0
        buy = prices[0]
        for i in range(1, len(prices)):
            if buy < prices[i]:
                profit += prices[i] - buy
            buy = prices[i]
        return profit
```

## Key Takeway
이 문제는 위 문제와 동일하지만 단지 구입과 판매를 여러번 할 수 있다는 조건이 붙을 뿐이다. 그러나 그렇게 됨으로써
의외로 문제는 간단해지는데, 왼쪽부터 오른쪽으로 가면서 이익이 될 수 있는 거래만 챙긴 뒤 그 이익을 더하면 되는 것이다.
이 케이스는 결국 local optimum의 합이 global optimum이라는 전제가 만족된다. 이러면 안전하게 Greedy algorithm을 쓸 수 있다.
Greedy algorithm을 쓰기 위해서는 (반복이 안되더라도) 최적 부분 구조가 존재해야하며, 한 번 내린 결정은 다시 돌아보지 않아야 한다.
이 경우 우리는 왼쪽에서 오른쪽으로 가며 각 부분에 이루어지는 거래에서 얻는 이득이 그 안에서 최적이기도 하다. 

이론적 베이스는 장황하지만 결국, 위와 같이 작은 이익을 더해감으로써 최종 이익을 얻을 수 있다.



# 55. Jump Game

## Solution
```python
class Solution(object):

```

## Key Takeway
