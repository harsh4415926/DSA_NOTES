# Problem: Matrix Chain Multiplication
You are given an array arr[] of N integers, where N-1 matrices are to be multiplied. The dimensions of the i-th matrix are given by arr[i-1] x arr[i]. Your task is to find the minimum number of scalar multiplication operations needed to compute the product of all matrices.

Example:

Input: arr = [10, 20, 30, 40]

Matrices: A1 (10x20), A2 (20x30), A3 (30x40)

Possible Parenthesizations:

(A1 * A2) * A3: (10*20*30) + (10*30*40) = 6000 + 12000 = 18000 operations.

A1 * (A2 * A3): (20*30*40) + (10*20*40) = 24000 + 8000 = 32000 operations.

Output: 18000

---
## memoization
````java
class Solution {
    // helper(i, j, ...) finds the min cost to multiply matrices from index i to j.
    public static int helper(int i, int j, int[] arr, int[][] dp) {
        // Base case: If there is only one matrix, no multiplication is needed.
        if (i == j) return 0;
        
        // Return the cached result if available.
        if (dp[i][j] != -1) return dp[i][j];
        
        int min = Integer.MAX_VALUE;
        // Try every possible split point 'k' for the chain from i to j.
        // This means partitioning as (M_i * ... * M_k) * (M_{k+1} * ... * M_j).
        for (int k = i; k < j; k++) {
            // Cost = (cost to multiply left part) + (cost to multiply right part)
            //        + (cost to multiply the two resulting matrices).
            // Dimensions of resulting matrices are (arr[i-1] x arr[k]) and (arr[k] x arr[j]).
            int steps = arr[i - 1] * arr[k] * arr[j] + helper(i, k, arr, dp) + helper(k + 1, j, arr, dp);
            min = Math.min(min, steps);
        }
        
        // Cache and return the minimum cost for this subproblem.
        return dp[i][j] = min;
    }

    static int matrixMultiplication(int arr[]) {
        int n = arr.length;
        // dp[i][j] will store the min cost to multiply matrices from M_i to M_j.
        int[][] dp = new int[n][n];
        for (int i = 0; i < n; i++) Arrays.fill(dp[i], -1);
        
        // The overall problem is to find the min cost for the full chain from M_1 to M_{n-1}.
        return helper(1, n - 1, arr, dp);
    }

    // Time Complexity: O(N^3)
    // - There are O(N^2) states, and for each state, the loop runs O(N) times.

    // Space Complexity: O(N^2)
    // - For the DP table and the O(N) recursion stack.
}
````
## tabulation
````java
class Solution {
    static int matrixMultiplication(int arr[]) {
        // code here
        int n = arr.length;
        int[][] dp = new int[n][n];
        for (int i = n - 1; i >= 1; i--) {
            for (int j = i; j <= n - 1; j++) {
                if (i == j) dp[i][j] = 0;
                else {
                    int min = Integer.MAX_VALUE;
                    for (int k = i; k < j; k++) {

                        int steps = arr[i - 1] * arr[k] * arr[j] + dp[i][k] + dp[k + 1][j];
                        min = Math.min(min, steps);

                    }
                    dp[i][j] = min;

                }
            }
        }
        return dp[1][n - 1];
    }
}
````