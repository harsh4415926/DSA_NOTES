````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 91 - Decode Ways
 * ----------------------------------------------------------------------
 * LOGIC (Pure Recursion / Decision Tree):
 * 1. The problem asks for the total number of ways to decode a string of 
 * digits into letters (1 -> 'A', 2 -> 'B', ..., 26 -> 'Z').
 * 2. At any given index 'i', we are faced with a choice (a branching path):
 * - Branch 1 (Single Digit): We can decode just the current digit, 
 * moving to index 'i + 1'.
 * - Branch 2 (Double Digit): We can decode the current and next digit 
 * together, moving to index 'i + 2'.
 * 3. Base Cases & Constraints:
 * - SUCCESS: If we reach the end of the string (i == s.length()), we 
 * found 1 valid complete decoding path.
 * - FAILURE: If the current digit is '0', it's an invalid path because 
 * there is no mapping for '0', nor can a valid two-digit mapping 
 * start with '0' (e.g., "06" is invalid).
 * - VALIDITY: The double-digit branch is only valid if the two digits 
 * form a number between 10 and 26 inclusive.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int numDecodings(String s) {
        return solve(0, s);
    }

    private int solve(int i, String s) {
        // Base Case 1: Successfully reached the end -> found 1 valid path
        if (i == s.length()) return 1;

        // Base Case 2: A string segment starting with '0' is completely invalid
        if (s.charAt(i) == '0') return 0;

        // Choice 1: Decode a single digit
        int ways = solve(i + 1, s);

        // Choice 2: Decode two digits (if we have at least 2 digits left)
        if (i + 1 < s.length()) {
            // Calculate the numerical value of the two-digit substring
            int num = (s.charAt(i) - '0') * 10 + (s.charAt(i + 1) - '0');
            
            // It is only a valid letter if it falls between 10 and 26 ('J' through 'Z')
            if (num >= 10 && num <= 26) {
                ways += solve(i + 2, s);
            }
        }

        return ways; // Total valid decodings from this point forward
    }
}

/*
 * COMPLEXITY:
 * Time: O(2^n) - Exponential. In the worst case (like "111111"), every single 
 * digit spawns two new recursive branches. Just like the Fibonacci/Stairs problems, 
 * this repeats the exact same subproblems and will cause a Time Limit Exceeded (TLE).
 * Space: O(n) - Maximum depth of the recursion call stack.
 */
````
