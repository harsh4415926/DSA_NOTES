# Problem: Number of Longest Increasing Subsequences
Given an integer array nums, your task is to find the number of strictly increasing subsequences that have the maximum possible length.

Example:

Input: nums = [1, 3, 5, 4, 7]

Output: 2

Explanation: The longest increasing subsequence has length 4. There are two such subsequences: [1, 3, 4, 7] and [1, 3, 5, 7].

---
## code
````java
class Solution {
    public int findNumberOfLIS(int[] nums) {
        int n = nums.length;
        // dp[i] stores the length of the LIS ending at index i.
        int[] dp = new int[n];
        // count[i] stores the number of LIS that end at index i.
        int[] count = new int[n];
        
        Arrays.fill(dp, 1);
        Arrays.fill(count, 1);
        
        // Tracks the length of the LIS found so far.
        int max = 0;
        // Tracks the total count of LIS with that max length.
        int maxCount = 0;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i]) {
                    // Case 1: Found a new, longer LIS ending at i.
                    if (1 + dp[j] > dp[i]) {
                        dp[i] = 1 + dp[j];
                        // Inherit the count from the previous subsequence.
                        count[i] = count[j];
                    }
                    // Case 2: Found another LIS of the same length ending at i.
                    else if (1 + dp[j] == dp[i]) {
                        // Add the counts of the previous subsequence.
                        count[i] += count[j];
                    }
                }
            }

            // After checking all previous elements, update the overall max and count.
            if (dp[i] > max) {
                max = dp[i]; // New max length found.
                maxCount = count[i]; // Reset the count.
            } else if (dp[i] == max) {
                maxCount += count[i]; // Found another LIS with the same max length.
            }
        }
        return maxCount;
    }

    // Time Complexity: O(N^2)
    // - Where N is the number of elements in nums.
    // - Due to the nested loops.

    // Space Complexity: O(N)
    // - For the 'dp' and 'count' arrays.
}
````
