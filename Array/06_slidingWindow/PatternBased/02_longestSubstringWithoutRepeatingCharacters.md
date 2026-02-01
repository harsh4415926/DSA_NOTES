# longest substring without repeating characters
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 3 - Longest Substring Without Repeating Characters
 * ----------------------------------------------------------------------
 * Find the length of the longest substring with unique characters.
 * * STEPS:
 * 1. Expand 'right' pointer adding chars to Set. If duplicate found -> Shrink 'left'.
 * 2. Remove chars from Set until duplicate is cleared -> Update Max Length.
 * ----------------------------------------------------------------------
 */

import java.util.HashSet;
import java.util.Set;

class Solution {
    public int lengthOfLongestSubstring(String s) {
        Set<Character> seen = new HashSet<>();
        int left = 0;
        int maxLength = 0;

        // Sliding Window: Expand right
        for (int right = 0; right < s.length(); right++) {
            char currentChar = s.charAt(right);

            // While duplicate exists in current window, shrink from left
            while (seen.contains(currentChar)) {
                seen.remove(s.charAt(left));
                left++;
            }

            // Add valid character to current window
            seen.add(currentChar);

            // Update global maximum
            maxLength = Math.max(maxLength, right - left + 1);
        }

        return maxLength;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N)
 * - Each character is added and removed at most once (2*N operations).
 * * Space Complexity: O(min(N, M))
 * - M is the size of the charset (e.g., 26 for letters, 128 for ASCII).
 * ----------------------------------------------------------------------
 */
````