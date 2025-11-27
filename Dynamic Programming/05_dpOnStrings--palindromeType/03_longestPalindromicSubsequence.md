# Problem: Longest Palindromic Subsequence
Given a string s, your task is to find the length of the longest palindromic subsequence. A subsequence is created by deleting zero or more characters from a string, and a palindrome reads the same forwards and backward.

The Key Insight:
You correctly identified that the longest palindromic subsequence of a string s is equivalent to the Longest Common Subsequence (LCS) between s and its reverse.

Example:

Input: s = "cbbd"

Reverse of s: "dbbc"

LCS("cbbd", "dbbc"): "bb"

Output: 2
___
## method 1(using lcs)
````

just find lcs for string s and its reverse
````
---
## recursion+memoization
````java
class Solution {
    // helper(i, j, ...) finds the LPS length in the substring s[i...j].
    public int helper(int i, int j, String s, int[][] dp) {
        // Base case: Empty range.
        if (i > j) return 0;
        // Base case: Single character is a palindrome of length 1.
        if (i == j) return 1;
        
        // Return cached result.
        if (dp[i][j] != -1) return dp[i][j];

        // If outer characters match...
        if (s.charAt(i) == s.charAt(j)) {
            // ...they are part of the palindrome. Add 2 and check the inner part.
            return dp[i][j] = 2 + helper(i + 1, j - 1, s, dp);
        }
        // If they don't match...
        else {
            // ...take the max of either skipping the 'i' char or the 'j' char.
            return dp[i][j] = Math.max(helper(i + 1, j, s, dp), helper(i, j - 1, s, dp));
        }
    }

    public int longestPalindromeSubseq(String s) {
        int n = s.length();
        // dp[i][j] stores the LPS length for s[i...j].
        int[][] dp = new int[n][n];
        for (int i = 0; i < n; i++) Arrays.fill(dp[i], -1);
        
        // Find the LPS for the entire string s[0...n-1].
        return helper(0, n - 1, s, dp);
    }

    // Time Complexity: O(N^2)
    // - Where N is the length of the string.
    // - There are O(N^2) states, and each is computed once.

    // Space Complexity: O(N^2)
    // - For the N x N DP table and the O(N) recursion stack.
}
````
## bottom up(using blue print)
````java
class Solution {
    public int longestPalindromeSubseq(String s) {
        int n = s.length();
        // dp[i][j] = length of the LPS in substring s[i...j].
        int[][] dp = new int[n][n];

        // Iterate over all possible lengths (L) of substrings.
        for (int L = 1; L <= n; L++) {
            // Iterate over all possible starting indices 'i'.
            for (int i = 0; i + L - 1 < n; i++) {
                int j = i + L - 1; // 'j' is the ending index.
                
                // Base case: Length 1 substring is a palindrome of length 1.
                if (L == 1) {
                    dp[i][j] = 1;
                }
                // If outer characters match...
                else if (s.charAt(i) == s.charAt(j)) {
                    // ...they are part of the palindrome. Add 2 + the inner LPS.
                    // (dp[i+1][j-1] is 0 if i+1 > j-1, which is correct).
                    dp[i][j] = 2 + dp[i + 1][j - 1];
                }
                // If they don't match...
                else {
                    // ...take the max of either skipping the 'i' char or the 'j' char.
                    dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
                }
            }
        }
        // The result is the LPS for the entire string s[0...n-1].
        return dp[0][n - 1];
    }

    // Time Complexity: O(N^2)
    // - Where N is the length of the string.
    // - We iterate through all O(N^2) substrings.

    // Space Complexity: O(N^2)
    // - For the N x N DP table.
}
````

