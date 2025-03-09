# Best Time to Buy and Sell Stock

## Problem Statement

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return `0`.

**Example 1:**
```
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
```

**Example 2:**
```
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transactions are done and the max profit = 0.
```

## Intuition

The key insight for this problem is to find the maximum difference between two prices, where the lower price comes before the higher price. We can solve this efficiently by keeping track of the minimum price seen so far and calculating the potential profit at each step.

As we iterate through the array, we update the minimum price if we find a lower price, and we update the maximum profit if selling at the current price would yield a higher profit.

## Visual Explanation

Let's visualize the solution with Example 1: `prices = [7,1,5,3,6,4]`

```
Step 1: Initialize min_price = infinity, max_profit = 0
        Current price: 7
        Update min_price = 7
        Potential profit = 7 - 7 = 0
        max_profit remains 0

Step 2: Current price: 1
        Update min_price = 1 (lower than previous min)
        Potential profit = 1 - 1 = 0
        max_profit remains 0

Step 3: Current price: 5
        min_price remains 1
        Potential profit = 5 - 1 = 4
        Update max_profit = 4

Step 4: Current price: 3
        min_price remains 1
        Potential profit = 3 - 1 = 2
        max_profit remains 4 (since 4 > 2)

Step 5: Current price: 6
        min_price remains 1
        Potential profit = 6 - 1 = 5
        Update max_profit = 5

Step 6: Current price: 4
        min_price remains 1
        Potential profit = 4 - 1 = 3
        max_profit remains 5 (since 5 > 3)

Result: max_profit = 5
```

![Buy Sell Stock Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Initialize two variables:
   - `min_price` to track the minimum price seen so far, initially set to infinity.
   - `max_profit` to track the maximum profit achievable, initially set to 0.
2. Iterate through the array of prices:
   - Update `min_price` if the current price is lower.
   - Calculate the potential profit if we sell at the current price: `current_price - min_price`.
   - Update `max_profit` if the potential profit is higher than the current `max_profit`.
3. Return `max_profit`.

## Code Implementation

### Python

```python
def maxProfit(prices):
    if not prices:
        return 0
    
    min_price = float('inf')
    max_profit = 0
    
    for price in prices:
        # Update min_price if we find a lower price
        min_price = min(min_price, price)
        
        # Calculate potential profit and update max_profit if higher
        potential_profit = price - min_price
        max_profit = max(max_profit, potential_profit)
    
    return max_profit
```

### JavaScript

```javascript
function maxProfit(prices) {
    if (!prices || prices.length === 0) {
        return 0;
    }
    
    let minPrice = Infinity;
    let maxProfit = 0;
    
    for (let i = 0; i < prices.length; i++) {
        // Update minPrice if we find a lower price
        minPrice = Math.min(minPrice, prices[i]);
        
        // Calculate potential profit and update maxProfit if higher
        const potentialProfit = prices[i] - minPrice;
        maxProfit = Math.max(maxProfit, potentialProfit);
    }
    
    return maxProfit;
}
```

## Time and Space Complexity

- **Time Complexity**: O(n), where n is the length of the prices array. We iterate through the array once.
- **Space Complexity**: O(1), as we only use a constant amount of extra space regardless of the input size.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 2: `prices = [7,6,4,3,1]`

```
Initialize min_price = Infinity, max_profit = 0

Step 1: price = 7
        Update min_price = 7
        potential_profit = 7 - 7 = 0
        max_profit remains 0

Step 2: price = 6
        Update min_price = 6
        potential_profit = 6 - 6 = 0
        max_profit remains 0

Step 3: price = 4
        Update min_price = 4
        potential_profit = 4 - 4 = 0
        max_profit remains 0

Step 4: price = 3
        Update min_price = 3
        potential_profit = 3 - 3 = 0
        max_profit remains 0

Step 5: price = 1
        Update min_price = 1
        potential_profit = 1 - 1 = 0
        max_profit remains 0

Result: max_profit = 0
```

In this example, the prices are in descending order, so we can never achieve a profit. The algorithm correctly returns 0.

## Alternative Approaches

### Brute Force Approach

A straightforward approach is to check all possible pairs of buy and sell days:

```python
def maxProfit(prices):
    max_profit = 0
    n = len(prices)
    
    for i in range(n):
        for j in range(i + 1, n):
            profit = prices[j] - prices[i]
            max_profit = max(max_profit, profit)
    
    return max_profit
```

This approach has a time complexity of O(n²) and a space complexity of O(1).

### Kadane's Algorithm Approach

We can also view this problem as finding the maximum subarray sum of the differences between consecutive prices:

```python
def maxProfit(prices):
    if not prices:
        return 0
    
    max_profit = 0
    current_profit = 0
    
    for i in range(1, len(prices)):
        # Calculate the price difference
        price_diff = prices[i] - prices[i - 1]
        
        # Update current profit using Kadane's algorithm
        current_profit = max(0, current_profit + price_diff)
        
        # Update max profit
        max_profit = max(max_profit, current_profit)
    
    return max_profit
```

This approach also has a time complexity of O(n) and a space complexity of O(1).

## Edge Cases and Optimizations

- **Empty Array**: If the input array is empty, the function will return 0.
- **Single Element**: If the array has only one element, no transaction can be made, so the function will return 0.
- **Decreasing Prices**: If the prices are in decreasing order, no profit can be made, so the function will return 0.
- **Equal Prices**: If all prices are the same, no profit can be made, so the function will return 0.

## Common Mistakes

1. **Not handling empty arrays**: Always check if the input array is empty.
2. **Using the wrong formula for profit**: Remember that profit is calculated as selling price minus buying price, not the other way around.
3. **Not considering the constraint that you must buy before you sell**: This is implicitly handled by our approach of tracking the minimum price seen so far.

## Related Problems

1. **Best Time to Buy and Sell Stock II**: You can make as many transactions as you like.
2. **Best Time to Buy and Sell Stock III**: You can complete at most two transactions.
3. **Best Time to Buy and Sell Stock IV**: You can complete at most k transactions.
4. **Best Time to Buy and Sell Stock with Cooldown**: After selling, you must wait one day before buying again. 