# Problem: Minimum Insertion Steps to Make a String Palindrome
Given a string s, the goal is to find the minimum number of characters you need to insert anywhere in the string to transform it into a palindrome.

The Key Insight:
You've correctly identified the core logic. The characters in the string that already form the longest palindromic subsequence (LPS) don't need to be touched. The characters that are not part of this subsequence are the ones that need a matching partner. Therefore, the number of insertions needed is simply the total length of the string minus the length of its LPS.

Example:

Input: s = "mbadm"

Length: 5

Longest Palindromic Subsequence: "mam" (length 3)

Characters not in LPS: 'b', 'd'

Insertions needed: We need to insert a 'b' and a 'd' to match them.

Formula: s.length() - LPS.length() = 5 - 3 = 2

---
## methode 1 (using longest palindromic subsequence)
just return length of string - longest palindromic subsequence
## code
````java
class Solution {
    // Helper function to find the length of the Longest Palindromic Subsequence (LPS).
    public int longestPalindromeSubseq(String s) {
        String s1 = s;
        // Create the reverse of the string s1.
        String s2 = new StringBuilder(s1).reverse().toString();

        int n = s1.length();
        // Use the standard bottom-up DP algorithm to find the LCS of s1 and its reverse.
        int[][] dp = new int[n + 1][n + 1];
        for (int i = 0; i < n + 1; i++) dp[i][0] = 0;
        for (int j = 0; j < n + 1; j++) dp[0][j] = 0;
        
        for (int i = 1; i < n + 1; i++) {
            for (int j = 1; j < n + 1; j++) {
                if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    dp[i][j] = Math.max(dp[i][j - 1], dp[i - 1][j]);
                }
            }
        }
        return dp[n][n]; // This is the length of the LPS.
    }

    public int minInsertions(String s) {
        // The number of insertions is the total length minus the part that's already palindromic.
        return s.length() - longestPalindromeSubseq(s);
    }
}
````
---

## recursion+memoization
````java
class Solution {
    // helper(i, j, ...) finds the min insertions to make s[i...j] a palindrome.
    public int helper(int i, int j, String s, int[][] dp) {
        // Base case: Empty or single-character string is already a palindrome.
        if (i >= j) return 0;
        
        // Return cached result.
        if (dp[i][j] != -1) return dp[i][j];

        // If outer characters match, no insertion is needed for them.
        // The cost is just the cost to make the inner part (i+1...j-1) a palindrome.
        if (s.charAt(i) == s.charAt(j)) {
            return dp[i][j] = helper(i + 1, j - 1, s, dp);
        }
        // If outer characters don't match...
        else {
            // ...we must add 1 (for the insertion) and find the minimum of:
            // 1. Making s[i+1...j] a palindrome (then add a char to match s[i]).
            // 2. Making s[i...j-1] a palindrome (then add a char to match s[j]).
            return dp[i][j] = 1 + Math.min(helper(i + 1, j, s, dp), helper(i, j - 1, s, dp));
        }
    }

    public int minInsertions(String s) {
        int n = s.length();
        // dp[i][j] stores the min insertions for s[i...j].
        int[][] dp = new int[n][n];
        for (int i = 0; i < n; i++) Arrays.fill(dp[i], -1);
        
        // Find the result for the entire string s[0...n-1].
        return helper(0, n - 1, s, dp);
    }

    // Time Complexity: O(N^2)
    // - Where N is the length of the string.
    // - There are O(N^2) states, and each is computed once.

    // Space Complexity: O(N^2)
    // - For the N x N DP table and the O(N) recursion stack.
}
````
## bottom up(using blueprint)
````java
class Solution {
    public int minInsertions(String s) {
        int n = s.length();
        // dp[i][j] = min insertions to make the substring s[i...j] a palindrome.
        int[][] dp = new int[n][n];

        // Iterate over all possible lengths (L) of substrings.
        for (int L = 1; L <= n; L++) {
            // Iterate over all possible starting indices 'i'.
            for (int i = 0; i + L - 1 < n; i++) {
                int j = i + L - 1; // 'j' is the ending index.
                
                // Base case: Length 1 is already a palindrome (0 insertions).
                if (L == 1) {
                    dp[i][j] = 0;
                }
                // If outer characters match...
                else if (s.charAt(i) == s.charAt(j)) {
                    // ...no insertions are needed for them. Cost is cost of inner part.
                    // (dp[i+1][j-1] is 0 if i+1 > j-1, which is correct).
                    dp[i][j] = dp[i + 1][j - 1];
                }
                // If outer characters don't match...
                else {
                    // ...we must add 1 insertion and take the min of the two subproblems.
                    dp[i][j] = 1 + Math.min(dp[i + 1][j], dp[i][j - 1]);
                }
            }
        }
        // The result is the cost for the entire string s[0...n-1].
        return dp[0][n - 1];
    }

    // Time Complexity: O(N^2)
    // - Where N is the length of the string.
    // - We iterate through all O(N^2) substrings.

    // Space Complexity: O(N^2)
    // - For the N x N DP table.
}
````


