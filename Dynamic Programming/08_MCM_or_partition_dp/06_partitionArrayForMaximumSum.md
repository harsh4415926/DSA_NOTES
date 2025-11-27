# Problem: Partition Array for Maximum Sum
You are given an integer array arr and an integer k. You must partition the array into contiguous subarrays, where each subarray has a length of at most k. After partitioning, the values of all elements in each subarray are changed to become the maximum value within that subarray. Your goal is to return the largest possible sum of the modified array.

Example:

Input: arr = [1, 15, 7, 9, 2, 5, 10], k = 3

Output: 84

Explanation:

The array can be partitioned into [1, 15, 7] and [9, 2, 5, 10].

This is invalid because the second partition [9, 2, 5, 10] has length 4, which is > k.

A valid partition is [1, 15, 7], [9, 2, 5], [10].

These become [15, 15, 15], [9, 9, 9], [10].

Sum = (15*3) + (9*3) + 10 = 45 + 27 + 10 = 82.

A better partition is [1, 15, 7], [9], [2, 5, 10].

These become [15, 15, 15], [9], [10, 10, 10].

Sum = (15*3) + 9 + (10*3) = 45 + 9 + 30 = 84.

---
## recursion+memoization
````java
class Solution {
    // helper(idx, ...) finds the max sum for the subarray arr[idx...end].
    public int helper(int idx, int k, int[] arr, int[] dp) {
        // Base case: If we are at the end of the array, no more sum can be added.
        if (idx == arr.length) return 0;
        
        // Return the cached result if available.
        if (dp[idx] != -1) return dp[idx];

        int max = Integer.MIN_VALUE; // Max element in the current partition.
        int maxSum = Integer.MIN_VALUE; // Max sum we can get from this 'idx'.

        // Try all possible partition lengths (from 1 to k).
        // 'i' is the end of the current partition.
        for (int i = idx; i < Math.min(arr.length, idx + k); i++) {
            // Find the max element in the current partition arr[idx...i].
            max = Math.max(max, arr[i]);
            
            // Calculate the sum for this partition:
            // (max * length) + (result of the rest of the array)
            int sum = max * (i - idx + 1) + helper(i + 1, k, arr, dp);
            
            // Update the max sum we've found starting from 'idx'.
            maxSum = Math.max(maxSum, sum);
        }
        
        // Cache and return the result.
        return dp[idx] = maxSum;
    }

    public int maxSumAfterPartitioning(int[] arr, int k) {
        int n = arr.length;
        // dp[i] stores the max sum for the subarray starting at index i.
        int[] dp = new int[n];
        Arrays.fill(dp, -1);
        
        // Start the recursion from the beginning of the array.
        return helper(0, k, arr, dp);
    }

    // Time Complexity: O(N * K)
    // - Where N is the array length.
    // - There are O(N) states, and for each state, the loop runs K times.

    // Space Complexity: O(N)
    // - For the DP array and the recursion stack.
}
````
