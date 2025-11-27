# Problem: Longest Common Substring
You are given two strings, S1 and S2. Your task is to find the length of the longest substring that is common to both strings. A substring is a contiguous (side-by-side) sequence of characters within a string.

This is different from a subsequence, as the characters in a substring must be consecutive.

Example:

Input: S1 = "ABCDGH", S2 = "ACDGHR"

Output: 4

Explanation: The longest common substring is "CDGH", which has a length of 4.

___
## code
````java
class Solution {
    public int longestCommonSubstr(String s1, String s2) {
        int n1 = s1.length();
        int n2 = s2.length();
        // DP table is one size larger to easily handle base cases.
        int[][] dp = new int[n1 + 1][n2 + 1];
        
        // Base cases (first row and column) are initialized to 0 by default in Java.
        for (int i = 0; i < n1 + 1; i++) dp[i][0] = 0;
        for (int j = 0; j < n2 + 1; j++) dp[0][j] = 0;
        
        // The answer can be anywhere in the table, so we need a variable to track the max.
        int max = 0;
        
        // Fill the DP table.
        for (int i = 1; i < n1 + 1; i++) {
            for (int j = 1; j < n2 + 1; j++) {
                
                // Case 1: The characters match.
                if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                    // The substring is extended. Value is 1 + the length of the previous one.
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } 
                // Case 2: The characters do not match.
                else {
                    // The contiguous substring is broken. Reset the count to 0.
                    dp[i][j] = 0;
                }
                
                // Update the overall maximum length found so far.
                max = Math.max(max, dp[i][j]);
            }
        }
        
        return max;
    }
}
````
