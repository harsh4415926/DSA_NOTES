# maximum points obtain from cards
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 1423 - Maximum Points You Can Obtain from Cards
 * ----------------------------------------------------------------------
 * Pick k cards from the start or end of the array to maximize the total score.
 * * STEPS:
 * 1. Initialize sum by taking all k cards from the left (start).
 * 2. Iteratively remove one card from left and pick one from right -> Update Max.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int maxScore(int[] cardPoints, int k) {
        int n = cardPoints.length;

        int lSum = 0;
        int rSum = 0;
        int maxSum = 0;

        // 1. Start by taking all k cards from the beginning
        for (int l = 0; l < k; l++) {
            lSum += cardPoints[l];
        }

        maxSum = lSum;
        int r = n - 1;

        // 2. Sliding Window: Shrink left window, Expand right window
        // We try: (k-1 left, 1 right), (k-2 left, 2 right) ... (0 left, k right)
        for (int l = k - 1; l >= 0; l--) {
            lSum -= cardPoints[l];       // Remove last card from left selection
            rSum += cardPoints[r];       // Add last card from array (right selection)
            maxSum = Math.max(maxSum, lSum + rSum);
            r--; // Move right pointer backwards
        }
        
        return maxSum;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(k)
 * - We iterate k times to build the initial sum, and k times to slide.
 * * Space Complexity: O(1)
 * - Only a few variables used for sums.
 * ----------------------------------------------------------------------
 */
````