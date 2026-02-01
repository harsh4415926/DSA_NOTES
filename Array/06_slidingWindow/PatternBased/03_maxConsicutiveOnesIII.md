# maximum consicutive ones III
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 1004 - Max Consecutive Ones III
 * ----------------------------------------------------------------------
 * Find the longest subarray of 1s allowing up to k zeros to be flipped.
 * * STEPS:
 * 1. Expand 'right' pointer. If num is 0, increment zero count.
 * 2. If zeros > k, move 'left' to shrink window. Update max length.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int longestOnes(int[] nums, int k) {
        int maxLen = 0;
        int zeroes = 0;
        int left = 0;
        
        for (int right = 0; right < nums.length; right++) {
            // 1. Expand window
            if (nums[right] == 0) zeroes++;

            // 2. Shrink window if invalid (zeros > k)
           
            while (zeroes > k) {
                if (nums[left] == 0) zeroes--;
                left++;
            }
            
            maxLen = Math.max(maxLen, right - left + 1);
        }
        return maxLen;
    }
}



// NOTE:  since we only need max length, we can 
// replace 'while' with 'if' here to keep the window size constant 
// (non-shrinking) rather than strictly valid at all times.
// That optimization maintains O(N) but reduces inner operations.



/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N)
 * - Standard (while): 'left' and 'right' traverse array once (2N steps).
 * - Optimized (if): 'left' only moves when needed, strict O(N).
 * * Space Complexity: O(1)
 * - Only integer variables used.
 * ----------------------------------------------------------------------
 */
````