# partition with given difference (gfg)
## Problem Statement
This code solves the "Count Partitions with a Given Difference" problem 📊. You're given an array of integers and a difference d. The task is to partition the array into two subsets such that the difference between their sums is exactly d. The code finds the total number of ways this can be done.
___
The core idea is a clever mathematical transformation. Instead of directly finding two subsets, the problem is converted into the "Count Subsets with a Given Sum" problem, which is then solved using dynamic programming.
## solution using the function of earlier function of counting number of subsets with target sum
````java
import java.util.Arrays;

class Solution {
    // Recursive helper to count subsets that sum up to 'sum'.
    public int helper(int idx, int[] arr, int sum, int[][] dp) {
        // Base case for the first element, handling zeros correctly.
        if (idx == 0) {
            // If sum is 0 and element is 0, there are two ways: pick it or not.
            if (sum == 0 && arr[idx] == 0) return 2;
            // If sum is 0 or the element equals the sum, there is one way.
            else if (sum == 0 || sum == arr[idx]) return 1;
            // Otherwise, it's impossible.
            return 0;
        }

        // Memoization: Return the pre-calculated result if it exists.
        if (dp[idx][sum] != -1) return dp[idx][sum];

        // Option 1: Include the current element (if possible).
        int take = 0;
        if (sum >= arr[idx]) take = helper(idx - 1, arr, sum - arr[idx], dp);

        // Option 2: Don't include the current element.
        int notTake = helper(idx - 1, arr, sum, dp);

        // Total ways = (ways by taking) + (ways by not taking).
        return dp[idx][sum] = take + notTake;
    }

    int countPartitions(int[] arr, int d) {
        int n = arr.length;
        // Calculate the total sum of all elements in the array.
        int totalSum = 0;
        for (int i = 0; i < n; i++) {
            totalSum += arr[i];
        }

        // This section reduces the problem to "Count Subsets with Sum".
        // We need S1 - S2 = d and S1 + S2 = totalSum.
        // This implies S1 = (d + totalSum) / 2.
        int sum = (d + totalSum) / 2;

        // If (d + totalSum) is odd, it's impossible to get an integer sum for S1.
        if ((d + totalSum) % 2 != 0) return 0;
        
        // This problem also implies that (d + totalSum) must be non-negative.
        if ((d + totalSum) < 0) return 0;


        // dp[i][j] will store the number of ways to get sum 'j' using elements up to 'i'.
        int[][] dp = new int[n][sum + 1];
        for (int i = 0; i < n; i++) Arrays.fill(dp[i], -1);
        
        // Call the helper to find the number of subsets that sum to the calculated target.
        return helper(n - 1, arr, sum, dp);
    }
    
    // --- Complexity Analysis ---
    // Time Complexity: O(N * target)
    // N is the number of elements, and target is the derived subset sum.
    
    // Space Complexity: O(N * target)
    // For the DP table and recursion stack.
}
````

