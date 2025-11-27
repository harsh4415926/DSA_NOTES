# Problem: Target Sum (LeetCode 494)
You are given an array of integers nums and an integer target. You need to find the number of different ways to assign either a + or a - symbol to each integer in the array so that the sum of the resulting expression equals the target.

Example:

Input: nums = [1, 1, 1, 1, 1], target = 3

Output: 5

Explanation: There are 5 ways to achieve a sum of 3:

+1+1+1+1-1 = 3

+1+1+1-1+1 = 3

+1+1-1+1+1 = 3

+1-1+1+1+1 = 3

-1+1+1+1+1 = 3
## memoization
````java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        // Entry point, calls the main logic.
        return countPartitions(nums, target);
    }

    // This is a standard "count of subsets with a given sum" helper.
    public int helper(int idx, int[] arr, int sum, int[][] dp) {

        // Base Case: We are at the first element (index 0).
        if (idx == 0) {
            // If target sum is 0 and the number is 0, we have 2 ways (+0 or -0/not pick).
            if (sum == 0 && arr[idx] == 0) return 2;
                // If target is 0 (must not pick arr[idx]) or equals arr[idx], there's 1 way.
            else if (sum == 0 || sum == arr[idx]) return 1;
            // Otherwise, it's impossible with this last number.
            return 0;
        }

        // If this state is already computed, return the result.
        if (dp[idx][sum] != -1) return dp[idx][sum];

        // Don't take the current element.
        int notTake = helper(idx - 1, arr, sum, dp);

        // Take the current element (if its value is less than or equal to the remaining sum).
        int take = 0;
        if (sum >= arr[idx]) {
            take = helper(idx - 1, arr, sum - arr[idx], dp);
        }

        // Total ways is the sum of taking and not taking the element.
        return dp[idx][sum] = take + notTake;
    }

    // Main logic function that transforms the problem.
    int countPartitions(int[] arr, int d) {
        int n = arr.length;
        int totalSum = 0;
        for (int i = 0; i < n; i++) {
            totalSum += arr[i];
        }

        // Deriving the target sum for our subproblem.
        int sum = (d + totalSum) / 2;

        // Edge Case: If (d + totalSum) is odd, no integer solution exists.
        if ((d + totalSum) % 2 != 0) return 0;
        // Edge Case: If required sum is negative, it's impossible.
        if (sum < 0) return 0;

        // DP table to memoize results.
        int[][] dp = new int[n][sum + 1];
        for (int i = 0; i < n; i++) Arrays.fill(dp[i], -1);

        // Solve the "count subsets with sum" subproblem.
        return helper(n - 1, arr, sum, dp);
    }
}
````


