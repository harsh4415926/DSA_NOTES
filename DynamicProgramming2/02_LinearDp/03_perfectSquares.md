````java
/*
* ----------------------------------------------------------------------
* PROBLEM: LeetCode 279 - Perfect Squares
* ----------------------------------------------------------------------
* LOGIC (Top-Down Dynamic Programming / Memoization):
* 1. The goal is to find the minimum number of perfect squares
* (1, 4, 9, 16...) that sum up to exactly 'n'.
* 2. From any given number 'n', we can transition to a smaller subproblem
* by subtracting any perfect square 'i*i' that is less than or equal to 'n'.
* 3. The recurrence relation is:
* dp[n] = 1 + min(dp[n - i*i]) for all valid i where i*i <= n.
* 4. We use a recursion tree to explore all paths. To prevent the massive
* redundancy of a pure recursive approach (which recalculates the same
* subproblems), we cache the result of each state 'n' in our 'dp' array.
* ----------------------------------------------------------------------
*/

import java.util.Arrays;

class Solution {

    public int helper(int n, int[] dp) {
        // Base case: If we've exactly hit 0, it takes 0 additional squares
        if (n == 0) return 0;

        // Memoization check: If we've already solved for 'n', return the cached result
        if (dp[n] != -1) return dp[n];
        
        // Initialize with a safely large number to find the minimum
        int minCount = (int) (1e9);

        // Try subtracting every possible perfect square less than or equal to 'n'
        for (int i = 1; i * i <= n; i++) {
            // 1 represents the single square (i*i) we are using right now.
            // helper(n - (i*i)) finds the optimal solution for the remaining remainder.
            int curr = 1 + helper(n - (i * i), dp);
            
            // Track the absolute minimum across all possible choices
            minCount = Math. Math.min(minCount, curr);
        }
        
        // Cache the result before returning it up the call stack
        return dp[n] = minCount;
    }
    
    public int numSquares(int n) {
        int[] dp = new int[n + 1];
        Arrays.fill(dp, -1); // Initialize DP array with -1 (uncalculated states)
        return helper(n, dp);
    }
}

/*
* COMPLEXITY:
* Time: O(N * sqrt(N)). For every number from 1 to N, we iterate through its
* square roots (which takes roughly sqrt(N) steps). Because of memoization,
* each state is calculated exactly once.
* Space: O(N) for the 'dp' array and the maximum depth of the recursion call stack.
  */
````