# Problem: Minimum Sum Partition(gfg)
You are given an array of integers. Your task is to divide this array into two subsets, S1 and S2, such that the absolute difference between the sum of their elements (abs(sum(S1) - sum(S2))) is as small as possible.

---

## Using recursion and dp

````java
// Integer[][] dp; 
// Initialize with nulls (or -1)

int solve(int index, int currentSum, int[] arr, int totalSum, Integer[][] dp) {

    // Base Case
    if (index == arr.length) {
        return Math.abs((2 * currentSum) - totalSum);
    }

    // DP Check
    if (dp[index][currentSum] != null) {
        return dp[index][currentSum];
    }

    // Choice 1: Include in S1
    int includeInS1 = solve(index + 1, currentSum + arr[index], arr, totalSum, dp);

    // Choice 2: Include in S2 or we can say not take in s1
    int includeInS2 = solve(index + 1, currentSum, arr, totalSum, dp);

    // Store and return the minimum
    dp[index][currentSum] = Math.min(includeInS1, includeInS2);
    return dp[index][currentSum];
}
````

## solving using subset sum function in qn 1
````java
class Solution {
    // Fills the DP table to find all possible subset sums.
    public void subsetSum(int[] nums, boolean[][] dp, int totalSum) {
        int n = nums.length;
        // Base case: A sum of 0 is always possible (by picking no elements).
        for (int i = 0; i < n; i++) dp[i][0] = true;
        
        // Base case: With only the first element, we can only make a sum equal to its value.
        if (nums[0] <= totalSum) dp[0][nums[0]] = true;

        // Fill the rest of the DP table.
        for (int i = 1; i < n; i++) {
            for (int target = 0; target <= totalSum; target++) {
                
                // Don't take the current element.
                boolean notTake = dp[i - 1][target];

                // Take the current element (if possible).
                boolean take = false;
                if (target >= nums[i]) {
                    take = dp[i - 1][target - nums[i]];
                }
                
                // A target is possible if we can either take or not take the current element.
                dp[i][target] = take || notTake;
            }
        }
    }

    public int minDifference(int[] nums) {
        int n = nums.length;
        int totalSum = 0;
        // Calculate the total sum of all elements in the array.
        for (int i = 0; i < n; i++) totalSum += nums[i];

        // dp[i][j] will be true if a subset with sum 'j' is possible using the first 'i' elements.
        boolean[][] dp = new boolean[n][totalSum + 1];
        
        // Populate the dp table with all achievable subset sums.
        subsetSum(nums, dp, totalSum);

        int min = Integer.MAX_VALUE;
        
        // Find the minimum difference. We only need to check up to half of the total sum.
        // If s1 is a possible sum, then s2 = totalSum - s1. We want to minimize abs(s1 - s2).
        for (int sum1 = 0; sum1 <= totalSum / 2; sum1++) {
            // Check if a subset with sum 'sum1' is possible using all elements.
            if (dp[n - 1][sum1]) {
                int sum2 = totalSum - sum1;
                int diff = Math.abs(sum1 - sum2);
                min = Math.min(min, diff);
            }
        }
        return min;
    }
}
````
