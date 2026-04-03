````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 96 - Unique Binary Search Trees
 * ----------------------------------------------------------------------
 * LOGIC (Top-Down DP / Catalan Numbers):
 * 1. The problem asks for the number of structurally unique BSTs that 
 * can be created using 'n' distinct nodes.
 * 2. If we pick a number 'i' (from 1 to n) to be the root of our tree:
 * - All numbers from 1 to i-1 must go to the LEFT subtree (size: i-1).
 * - All numbers from i+1 to n must go to the RIGHT subtree (size: n-i).
 * 3. The total unique BSTs with 'i' as the root is the product of the 
 * unique ways to arrange the left subtree AND the right subtree (left * right).
 * 4. We sum these possibilities for EVERY possible root 'i' from 1 to n.
 * 5. Since the number of unique trees only depends on the *count* of nodes 
 * (not their actual values), we memoize the results to avoid recalculating 
 * the same tree sizes repeatedly.
 * ----------------------------------------------------------------------
 */

import java.util.Arrays;

class Solution {

    public int numTrees(int n) {
        int[] dp = new int[n + 1];
        Arrays.fill(dp, -1); // Initialize memoization array with -1
        return solve(n, dp);
    }

    public int solve(int n, int[] dp) {
        // Base case: A tree with 0 nodes (empty) or 1 node has exactly 1 unique structure
        if (n == 0 || n == 1) return 1;

        // Memoization check: Return immediately if we've already calculated for 'n' nodes
        if (dp[n] != -1) return dp[n];

        int ans = 0;

        // Try making every number from 1 to 'n' the root of the tree
        for (int i = 1; i <= n; i++) {
            
            // Calculate unique structural combinations for the left and right subtrees
            int left = solve(i - 1, dp);
            int right = solve(n - i, dp);

            // Multiply them because every unique left subtree can be paired 
            // with every unique right subtree (Cartesian product)
            ans += left * right;
        }

        // Cache the result for this subset size before returning
        return dp[n] = ans;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N^2). We solve for 'N' states. For each state 'n', we run a loop of size 'n'.
 * Space: O(N) for the 'dp' array and the recursion call stack.
 */
````

## using Catalan no.
````java
