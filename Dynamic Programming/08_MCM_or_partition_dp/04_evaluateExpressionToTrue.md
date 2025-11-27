# Problem: Evaluate Expression to True(interviewbit)  [link](https://www.interviewbit.com/problems/evaluate-expression-to-true/)
You are given a string s representing a boolean expression with operands (T for True, F for False) and operators (& for AND, | for OR, ^ for XOR). Your task is to find the number of ways to parenthesize the expression such that it evaluates to true. The result should be returned modulo 1003.

Example:

Input: s = "T|F&T"

Output: 2

Explanation:

(T | F) & T = T & T = T

T | (F & T) = T | F = T There are 2 ways to get true.

---
## memoization 
````java
public class Solution {
    private static final int MOD = 1003; // Given modulus
    // dp[i][j][isTrue] stores the number of ways for substring i..j
    // to evaluate to 'true' (if isTrue=1) or 'false' (if isTrue=0).
    private int[][][] dp;

    // helper(i, j, ...) finds ways for the expression from index i to j.
    private int helper(int i, int j, int isTrue, String s) {
        // Base case: Empty sub-expression has 0 ways.
        if (i > j) return 0;
        
        // Base case: Single operand (T or F).
        if (i == j) {
            if (isTrue == 1) return s.charAt(i) == 'T' ? 1 : 0;
            else return s.charAt(i) == 'F' ? 1 : 0;
        }
        
        // Return cached result.
        if (dp[i][j][isTrue] != -1) return dp[i][j][isTrue];

        long ways = 0L; // Use long to prevent overflow before modulo.
        // Iterate over all operators (k) as split points.
        for (int idx = i + 1; idx <= j - 1; idx += 2) {
            // Get all 4 sub-results: Left-True, Left-False, Right-True, Right-False.
            int lt = helper(i, idx - 1, 1, s);
            int lf = helper(i, idx - 1, 0, s);
            int rt = helper(idx + 1, j, 1, s);
            int rf = helper(idx + 1, j, 0, s);
            char op = s.charAt(idx);

            // Apply boolean logic based on the operator.
            if (op == '&') {
                if (isTrue == 1) {
                    ways += (long) lt * rt; // T & T = T
                } else {
                    ways += (long) lf * rf + (long) lt * rf + (long) lf * rt; // F&F, T&F, F&T = F
                }
            } else if (op == '|') {
                if (isTrue == 1) {
                    ways += (long) lt * rt + (long) lt * rf + (long) lf * rt; // T|T, T|F, F|T = T
                } else {
                    ways += (long) lf * rf; // F | F = F
                }
            } else if (op == '^') {
                if (isTrue == 1) {
                    ways += (long) lt * rf + (long) lf * rt; // T^F, F^T = T
                } else {
                    ways += (long) lt * rt + (long) lf * rf; // T^T, F^F = F
                }
            }
            ways %= MOD; // Apply modulo in each step.
        }
        
        // Cache and return the result.
        dp[i][j][isTrue] = (int) (ways % MOD);
        return dp[i][j][isTrue];
    }

    public int cnttrue(String A) {
        int n = A.length();
        dp = new int[n][n][2];
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                dp[i][j][0] = dp[i][j][1] = -1;
        
        // Start recursion for the full string, wanting a 'true' result.
        return helper(0, n - 1, 1, A);
    }
    
    // Time Complexity: O(N^3)
    // - Where N is the length of the string.
    // - O(N^2) states, and the inner loop 'k' runs O(N) times.

    // Space Complexity: O(N^2)
    // - For the O(N^2 * 2) DP table and O(N) recursion stack.
}
````
