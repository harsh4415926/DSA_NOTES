# number of strings containing all three characters
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 1358 - Number of Substrings Containing All Three Characters
 * ----------------------------------------------------------------------
 * Count the number of substrings containing at least one 'a', one 'b', and one 'c'.
 * * STEPS:
 * 1. Expand 'right' window. Once valid (all 3 present) -> Add (n - right) to answer.
 * 2. Why (n - right)? If s[left...right] is valid, then s[left...right+1] etc. are also valid.
 * 3. Shrink 'left' and repeat to find other valid starting positions.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int numberOfSubstrings(String s) {
        int n = s.length();
        int[] count = new int[3]; // Stores count of 'a', 'b', 'c'
        int left = 0;
        int ans = 0;

        for (int right = 0; right < n; right++) {
            // Update count for current character
            count[s.charAt(right) - 'a']++;

            // While window is valid (contains at least one 'a', 'b', and 'c')
            while (count[0] > 0 && count[1] > 0 && count[2] > 0) {
                // Key Logic:
                // Since the substring from 'left' to 'right' is valid,
                // extending this substring to the end of the string (right+1, right+2... n-1)
                // will ALSO be valid.
                // Number of such substrings = (last_index - current_right + 1) = n - right.
                ans += (n - right);

                // Shrink window from left to check the next potential starting position
                count[s.charAt(left) - 'a']--;
                left++;
            }
        }
        return ans;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N)
 * - Both 'left' and 'right' pointers move across the string at most once.
 * * Space Complexity: O(1)
 * - Uses a fixed-size integer array of length 3.
 * ----------------------------------------------------------------------
 */
````