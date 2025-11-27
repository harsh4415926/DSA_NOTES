# Problem: Best Time to Buy and Sell Stock
You are given an array prices where prices[i] is the price of a given stock on the i-th day. You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock. Return the maximum profit you can achieve. If you cannot achieve any profit, return 0.

Example:

Input: prices = [7, 1, 5, 3, 6, 4]

Output: 5

Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6 - 1 = 5.

## code
````java
class Solution {
    public int maxProfit(int[] prices) {
        // Initialize 'min' to the price on the first day.
        int min = prices[0];
        // Initialize 'profit' to 0, as it's the minimum possible profit.
        int profit = 0;

        // Iterate through the prices starting from the second day.
        for (int i = 1; i < prices.length; i++) {
            // Calculate the potential profit if we were to sell on the current day 'i'.
            int tempProfit = prices[i] - min;
            
            // Update the overall maximum profit if the current profit is higher.
            profit = Math.max(tempProfit, profit);
            
            // Update the minimum price seen so far for future calculations.
            min = Math.min(min, prices[i]);
        }
        return profit;
    }

    // Time Complexity: O(N)
    // - Where N is the number of days (the length of the prices array).
    // - We iterate through the array only once.

    // Space Complexity: O(1)
    // - We only use a few constant variables (min, profit, tempProfit)
    // - and do not use any extra space that scales with the input size.
}
````


