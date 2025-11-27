## using shortestCommonSubsequence dp table(tabulation)
````java
class Solution {
    public String shortestCommonSupersequence(String str1, String str2) {
        int n1 = str1.length();
        int n2 = str2.length();
        // dp[i][j] stores the length of the SCS for str1[0..i-1] and str2[0..j-1].
        int[][] dp = new int[n1 + 1][n2 + 1];

        // Part 1: Fill the DP table with SCS lengths.
        for (int i = 0; i < n1 + 1; i++) dp[i][0] = i; // Base case: SCS with empty string.
        for (int j = 0; j < n2 + 1; j++) dp[0][j] = j; // Base case: SCS with empty string.

        for (int i = 1; i < n1 + 1; i++) {
            for (int j = 1; j < n2 + 1; j++) {
                // If characters match, they contribute 1 to the SCS length.
                if (str1.charAt(i - 1) == str2.charAt(j - 1)) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                }
                // If not, add 1 to the minimum of the two subproblems.
                else {
                    dp[i][j] = 1 + Math.min(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }

        // Part 2: Backtrack from the bottom-right to build the SCS string.
        int i = n1;
        int j = n2;
        StringBuilder ans = new StringBuilder();

        while (i > 0 && j > 0) {
            // If characters are the same, they are part of the SCS. Add once.
            if (str1.charAt(i - 1) == str2.charAt(j - 1)) {
                ans.append(str1.charAt(i - 1));
                i--;
                j--;
            }
            // If the top cell's value is smaller, it means str1[i-1] was added.
            else if (dp[i - 1][j] < dp[i][j - 1]) {
                ans.append(str1.charAt(i - 1));
                i--;
            }
            // Otherwise, the left cell's value was smaller, so str2[j-1] was added.
            else {
                ans.append(str2.charAt(j - 1));
                j--;
            }
        }

        // Add any remaining characters from str1 or str2.
        while (i > 0) {
            ans.append(str1.charAt(i - 1));
            i--;
        }
        while (j > 0) {
            ans.append(str2.charAt(j - 1));
            j--;
        }
        
        // The string was built backwards, so reverse it for the final answer.
        return new String(ans.reverse());
    }

    // Time Complexity: O(N * M)
    // - Where N and M are the lengths of the strings.
    // - O(N * M) to fill the DP table and O(N + M) to backtrack.

    // Space Complexity: O(N * M)
    // - For the N x M DP table.
}
 ````

## using lcs dp table(tabulation)
````java
class Solution {
    public String shortestCommonSupersequence(String str1, String str2) {
        int n1 = str1.length();
        int n2 = str2.length();
        int[][] dp = new int[n1 + 1][n2 + 1];

        // Part 1: Fill the DP table with the lengths of the LCS.
        for (int i = 0; i < n1 + 1; i++) dp[i][0] = 0;
        for (int j = 0; j < n2 + 1; j++) dp[0][j] = 0;
        for (int i = 1; i < n1 + 1; i++) {
            for (int j = 1; j < n2 + 1; j++) {
                if (str1.charAt(i - 1) == str2.charAt(j - 1)) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }

        // Part 2: Backtrack through the LCS table to build the SCS string.
        int i = n1;
        int j = n2;
        StringBuilder ans = new StringBuilder();
        while (i > 0 && j > 0) {
            // If characters match, they are part of the LCS. Add once.
            if (str1.charAt(i - 1) == str2.charAt(j - 1)) {
                ans.append(str1.charAt(i - 1));
                i--;
                j--;
            }
            // If not, move towards the larger LCS subproblem and add the unique char.
            else if (dp[i - 1][j] > dp[i][j - 1]) {
                ans.append(str1.charAt(i - 1));
                i--;
            } else {
                ans.append(str2.charAt(j - 1));
                j--;
            }
        }
        
        // Add any leftover characters from the start of the strings.
        while (i > 0) {
            ans.append(str1.charAt(i - 1));
            i--;
        }
        while (j > 0) {
            ans.append(str2.charAt(j - 1));
            j--;
        }
        
        // Reverse the result as it was built backwards.
        return new String(ans.reverse());
    }

    // Time Complexity: O(N * M)
    // - Where N and M are the lengths of the strings.
    // - O(N * M) to fill the DP table and O(N + M) to backtrack.

    // Space Complexity: O(N * M)
    // - For the N x M DP table.
}
````