# Problem: Wildcard Matching(leetcode)
Given an input string s and a pattern p, implement wildcard pattern matching with support for ? and *.

? Matches any single character.

* Matches any sequence of characters (including the empty sequence).

The matching should cover the entire input string (not partial).

Example:

Input: s = "adceb", p = "*a*b"

Output: true

Explanation: The first * matches the empty sequence, while the second * matches the substring "dce".

---

## memoization
````java
class Solution {
    public boolean helper(int i, int j, String s, String p, int[][] dp) {
        // Base case: Both strings are fully matched.
        if (i < 0 && j < 0) return true;
        
        // Base case: String 's' is exhausted, check if remaining pattern 'p' is all '*'.
        if (i < 0) {
            for (int idx = 0; idx <= j; idx++) {
                if (p.charAt(idx) != '*') return false;
            }
            return true;
        }
        
        // Base case: Pattern 'p' is exhausted, but string 's' is not.
        if (j < 0) return false;
        
        // Return cached result (1 for true, 0 for false).
        if (dp[i][j] != -1) return dp[i][j] == 1;

        boolean result = false;
        // If characters match or pattern has '?', they are a match.
        if (s.charAt(i) == p.charAt(j) || p.charAt(j) == '?') {
            result = helper(i - 1, j - 1, s, p, dp);
        }
        // If pattern has a '*'.
        else if (p.charAt(j) == '*') {
            // '*' can match an empty sequence (j-1) OR match one/more characters (i-1).
            // The user's code has a redundant check for (i-1, j-1).
            boolean emptyMatch = helper(i, j - 1, s, p, dp);
            boolean charMatch = helper(i - 1, j, s, p, dp);
            result = emptyMatch || charMatch;
        }
        // If characters don't match and it's not a wildcard.
        else {
            result = false;
        }

        // Cache the result before returning.
        dp[i][j] = result ? 1 : 0;
        return result;
    }

    public boolean isMatch(String s, String p) {
        int n1 = s.length();
        int n2 = p.length();
        int[][] dp = new int[n1][n2];
        for (int i = 0; i < n1; i++) Arrays.fill(dp[i], -1);
        
        return helper(n1 - 1, n2 - 1, s, p, dp);
    }

    // Time Complexity: O(N * M)
    // - Where N and M are the lengths of the string and pattern.
    // - Each state (i, j) is computed once.

    // Space Complexity: O(N * M)
    // - For the DP table and the recursion stack.
}
````
## tabulation
````java
class Solution {
    public boolean isMatch(String s, String p) {
        int n1 = s.length();
        int n2 = p.length();
        // dp[i][j] = true if s[0..i-1] matches p[0..j-1].
        boolean[][] dp = new boolean[n1 + 1][n2 + 1];

        // Iterate through all states.
        for (int i = 0; i < n1 + 1; i++) {
            for (int j = 0; j < n2 + 1; j++) {
                // Base case: Empty string matches empty pattern.
                if (i == 0 && j == 0) dp[i][j] = true;
                // Base case: Empty string 's' only matches a pattern of all '*'.
                else if (i == 0) {
                    boolean flag = true;
                    for (int idx = 1; idx <= j; idx++) {
                        if (p.charAt(idx - 1) != '*') {
                            flag = false;
                            break;
                        }
                    }
                    dp[i][j] = flag;
                }
                // Base case: A non-empty 's' cannot match an empty pattern.
                else if (j == 0) {
                    dp[i][j] = false;
                }
                // Case 1: Characters match. Result depends on the rest of the strings.
                else if (s.charAt(i - 1) == p.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                }
                // Case 2: Pattern has '?', which acts as a match.
                else if (p.charAt(j - 1) == '?') {
                    dp[i][j] = dp[i - 1][j - 1];
                }
                // Case 3: Pattern has '*'.
                else if (p.charAt(j - 1) == '*') {
                    // '*' can either match an empty sequence (dp[i][j-1])
                    // or match one/more characters (dp[i-1][j]).
                    dp[i][j] = dp[i][j - 1] || dp[i - 1][j];
                }
                // Case 4: Characters don't match and it's not a wildcard.
                else {
                    dp[i][j] = false;
                }
            }
        }
        return dp[n1][n2];
    }
    
    // Time Complexity: O(N * M)
    // - Where N and M are the lengths of the string and pattern.
    // - Due to the nested loops filling the DP table.

    // Space Complexity: O(N * M)
    // - For the N x M DP table.
}
````
