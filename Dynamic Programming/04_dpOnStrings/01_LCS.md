# Problem: Longest Common Subsequence (LCS)
You are given two strings, text1 and text2. Your goal is to find the length of their longest common subsequence. A subsequence is formed by deleting some (or no) characters from a string without changing the order of the remaining characters.

Example:

Input: text1 = "abcde", text2 = "ace"

Output: 3

Explanation: The longest subsequence that appears in both strings is "ace", which has a length of 3.

---
## memoization
````java
class Solution {
    // Helper function now takes String objects directly.
    public int helper(int idx1, int idx2, String s1, String s2, int[][] dp) {
        // Base Case: If either string's index is out of bounds, no subsequence.
        if (idx1 < 0 || idx2 < 0) return 0;

        // If subproblem is already solved, return the stored result.
        if (dp[idx1][idx2] != -1) return dp[idx1][idx2];

        // Case 1: The characters at the current indices match.
        if (s1.charAt(idx1) == s2.charAt(idx2)) {
            // Result is 1 + the LCS of the remaining parts of both strings.
            return dp[idx1][idx2] = 1 + helper(idx1 - 1, idx2 - 1, s1, s2, dp);
        }
        
        // Case 2: Characters do not match.
        // Find the maximum LCS by exploring two possibilities.
        return dp[idx1][idx2] = Math.max(
            helper(idx1, idx2 - 1, s1, s2, dp), 
            helper(idx1 - 1, idx2, s1, s2, dp)
        );
    }

    public int longestCommonSubsequence(String text1, String text2) {
        int n1 = text1.length();
        int n2 = text2.length();
        // dp[i][j] stores the LCS length for text1[0..i] and text2[0..j].
        int[][] dp = new int[n1][n2];
        for (int i = 0; i < n1; i++) Arrays.fill(dp[i], -1);

        // Call the helper function with the original strings.
        return helper(n1 - 1, n2 - 1, text1, text2, dp);
    }
}
````
## tabulation
````java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int n1 = text1.length();
        int n2 = text2.length();
        // DP table is one size larger to easily handle base cases (empty strings).
        // dp[i][j] = LCS of text1's first 'i' chars and text2's first 'j' chars.
        int[][] dp = new int[n1 + 1][n2 + 1];
        
        // Note: Using StringBuilder is not necessary.
        StringBuilder s1 = new StringBuilder(text1);
        StringBuilder s2 = new StringBuilder(text2);
        
        // Initialize the base cases (first row and column).
        // Note: This is done automatically in Java as default int value is 0.
        for (int idx1 = 0; idx1 < n1 + 1; idx1++) dp[idx1][0] = 0;
        for (int idx2 = 0; idx2 < n2 + 1; idx2++) dp[0][idx2] = 0;
        
        // Fill the DP table starting from index 1.
        for (int i = 1; i < n1 + 1; i++) {
            for (int j = 1; j < n2 + 1; j++) {
                
                // Case 1: The characters match.
                // We use i-1 and j-1 because strings are 0-indexed while our DP table is 1-indexed.
                if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                    // The result is 1 + the LCS of the strings without these characters.
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } 
                // Case 2: The characters do not match.
                else {
                    // The result is the max LCS from either ignoring s1's char or s2's char.
                    dp[i][j] = 0 + Math.max(dp[i][j - 1], dp[i - 1][j]);
                }
            }
        }
        
        // The final answer is in the bottom-right cell of the table.
        return dp[n1][n2];
    }
}
````
