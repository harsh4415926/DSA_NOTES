# longest repeating character replacement 
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 424 - Longest Repeating Character Replacement
 * ----------------------------------------------------------------------
 * Find the longest substring where you can replace at most 'k' characters 
 * to make the whole substring consist of the same character.
 * * STEPS:
 * 1. Expand 'right' & track Count of most frequent char (`maxFreq`) in window.
 * 2. Valid Condition: (Window Length - `maxFreq`) <= k.
 * (i.e., The number of "other" characters to replace is within limit k).
 * 3. If invalid, shrink 'left'.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int characterReplacement(String s, int k) {
        int n = s.length();
        int left = 0;
        int maxFreq = 0;
        int maxLen = 0;
        int[] freq = new int[26];
       
       for (int right = 0; right < n; right++) {
            int chr = s.charAt(right) - 'A';
            freq[chr]++;
            
            // Track the count of the dominant character in the current window.
            // This allows us to maximize the valid window size: maxFreq + k.
            maxFreq = Math.max(maxFreq, freq[chr]);
            
            // Check if window is invalid:
            // (Total Chars) - (Dominant Chars) = (Chars needing replacement)
            // If replacements needed > k, the window is invalid.
            while ((right - left + 1) - maxFreq > k) {
                int chl = s.charAt(left) - 'A';
                freq[chl]--;
                
                // NOTE:
                // You do NOT need to recalculate maxFreq here.
                // The algorithm works because we only care about expanding maxLen.
                // If maxFreq decreases, the window size constraint (len - maxFreq <= k)
                // becomes tighter, so we won't find a new maximum anyway.
                // We only need to update maxFreq if we find a higher frequency.
                
                left++;
            }
            
            // If valid, update max length
            if ((right - left + 1) - maxFreq <= k) {
                maxLen = Math.max(maxLen, (right - left + 1));
            }
       }
       return maxLen;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N)
 * - 'right' and 'left' pointers traverse the string once. 
 * - Avoiding the loop to recalculate maxFreq keeps the inner logic O(1).
 * * Space Complexity: O(1)
 * - Fixed size array of 26 integers.
 * ----------------------------------------------------------------------
 */
````
