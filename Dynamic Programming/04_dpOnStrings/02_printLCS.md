# problem-
Given two strings, text1 and text2, the goal is to find and print one of the actual longest common subsequence strings. This involves first calculating the lengths of the subsequences in a DP table and then backtracking through the table to build the result string.

Example:

Input: text1 = "acbcf", text2 = "abcdaf"

Output: "abcf"
 ___
The solution is a two-step process:

longestCommonSubsequence: A helper function that fills a DP table with the lengths of the LCS, using the same bottom-up approach as before.

printLCS: The main function that uses the completed DP table to backtrack and reconstruct the LCS string.
## code
````java
class Solution {
    // This function fills the DP table with LCS lengths, same as the previous solution.
    public static void longestCommonSubsequence(int[][] dp, StringBuilder s1, StringBuilder s2, int n1, int n2) {
        // Base cases (first row/col) are implicitly 0 in Java.
        for (int idx1 = 0; idx1 < n1 + 1; idx1++) dp[idx1][0] = 0;
        for (int idx2 = 0; idx2 < n2 + 1; idx2++) dp[0][idx2] = 0;

        // Fill the rest of the table.
        for (int i = 1; i < n1 + 1; i++) {
            for (int j = 1; j < n2 + 1; j++) {
                // If characters match, add 1 to the diagonal's result.
                if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } 
                // If they don't match, take the max from the top or left cell.
                else {
                    dp[i][j] = 0 + Math.max(dp[i][j - 1], dp[i - 1][j]);
                }
            }
        }
    }

    // This function reconstructs and prints the LCS string using the filled DP table.
    public static void printLCS(String text1, String text2) {
        int n1 = text1.length();
        int n2 = text2.length();
        StringBuilder s1 = new StringBuilder(text1);
        StringBuilder s2 = new StringBuilder(text2);
        int[][] dp = new int[n1 + 1][n2 + 1];
        
        // 1. Fill the DP table with LCS lengths.
        longestCommonSubsequence(dp, s1, s2, n1, n2);

        // 2. Backtrack from the bottom-right corner of the table to build the string.
        int i = n1;
        int j = n2;
        StringBuilder ans = new StringBuilder();
        
        while (i > 0 && j > 0) {
            // If the characters are the same, they are part of the LCS.
            if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                ans.append(s1.charAt(i - 1)); // Add the character to our answer.
                i--; // Move diagonally up-left in the table.
                j--;
            } 
            // If not same, move in the direction of the larger subproblem result.
            else if (dp[i - 1][j] > dp[i][j - 1]) {
                i--; // Move up.
            } else {
                j--; // Move left.
            }
        }
        
        // The answer string is built in reverse, so we need to reverse it at the end.
        System.out.println(ans.reverse().toString());
    }
}
````
