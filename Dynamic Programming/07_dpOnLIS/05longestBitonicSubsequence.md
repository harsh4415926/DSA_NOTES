# Problem: Longest Bitonic Subsequence
Given an array of integers nums, your task is to find the length of the longest bitonic subsequence. A bitonic subsequence is one that first strictly increases and then may strictly decrease. A sequence that is purely increasing or purely decreasing is also considered bitonic.

Example:

Input: nums = [1, 11, 2, 10, 4, 5, 2, 1]

Output: 6

Explanation: The longest bitonic subsequence is [1, 2, 4, 5, 2, 1].

---
## code
````java
class Solution {
    public static int LongestBitonicSequence(int n, int[] nums) {
        // dpInc[i] stores the length of the LIS ending at index i.
        int[] dpInc = new int[n];
        // dpDec[i] stores the length of the LDS starting at index i.
        int[] dpDec = new int[n];
        Arrays.fill(dpInc, 1);
        Arrays.fill(dpDec, 1);

        // 1. Calculate the Longest Increasing Subsequence (LIS) from left to right.
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i]) {
                    dpInc[i] = Math.max(1 + dpInc[j], dpInc[i]);
                }
            }
        }

        // 2. Calculate the Longest Decreasing Subsequence (LDS) from right to left.
        // This is the same as finding the LIS on the reversed array.
        for (int i = n - 1; i >= 0; i--) {
            for (int j = n - 1; j > i; j--) {
                if (nums[j] < nums[i]) {
                    dpDec[i] = Math.max(1 + dpDec[j], dpDec[i]);
                }
            }
        }
        
        // 3. Find the maximum bitonic length.
        int max = 0;
        for (int i = 0; i < n; i++) {
            // The length of the bitonic sequence with nums[i] as the peak is
            // (LIS ending at i) + (LDS starting at i) - 1 (to not double-count the peak).
            // The original 'if' condition here was removed as it incorrectly excluded
            // purely increasing or decreasing sequences.
            int bitonicLength = dpInc[i] + dpDec[i] - 1;
            max = Math.max(max, bitonicLength);
        }
        return max;
    }

    // Time Complexity: O(N^2)
    // - Where N is the number of elements.
    // - Two separate O(N^2) loops are used, which dominate the runtime.

    // Space Complexity: O(N)
    // - For the 'dpInc' and 'dpDec' arrays.
}
````




