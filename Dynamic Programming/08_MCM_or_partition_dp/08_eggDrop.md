# super egg drop (leetcode)

````java


class Solution {

    /**
     * Helper function with memoization.
     * dp[k][n] = min moves for k eggs, n floors.
     */
    public int helper(int k, int n, int[][] dp) {
        // --- Base Cases ---

        // if(k==0)return Integer.MIN_VALUE; // This case is never hit due to k==1 check
        
        // If we only have 1 egg, we must drop 1 by 1 from floor 1.
        // Worst case is n moves.
        if (k == 1) return n;
        
        // If there are 0 floors, 0 moves needed.
        if (n == 0) return 0;
        
        // If there is 1 floor, 1 move needed.
        if (n == 1) return 1;

        // --- Memoization Check ---
        if (dp[k][n] != -1) return dp[k][n];

        int minMoves = Integer.MAX_VALUE;
        
        // Binary search for the optimal floor 'mid' to drop from
        int low = 1, high = n;
        while (low <= high) {
            int mid = low + (high - low) / 2; // 'mid' is the floor 'f' we test

            // --- Calculate worst-case outcomes for dropping at 'mid' ---

            // Case 1: Egg breaks at 'mid'.
            // We lose 1 egg (k-1) and must check floors below (mid-1).
            int eggBreak = helper(k - 1, mid - 1, dp);
            
            // Case 2: Egg does not break at 'mid'.
            // We keep k eggs and must check floors above (n-mid).
            int eggNotBreak = helper(k, n - mid, dp);

            // The worst case is the max of the two outcomes, +1 for this drop.
            int worstCaseMoves = 1 + Math.max(eggBreak, eggNotBreak);

            // We want to MINIMIZE this worst-case move count.
            minMoves = Math.min(minMoves, worstCaseMoves);
            
            // --- Adjust Binary Search ---
            // We are looking for the 'mid' where eggBreak and eggNotBreak are closest.
            
            if (eggBreak > eggNotBreak) {
                // 'eggBreak' cost is higher, our 'mid' is too high.
                // Move left to decrease 'eggBreak' cost and increase 'eggNotBreak' cost.
                high = mid - 1;
            } else {
                // 'eggNotBreak' cost is higher (or equal).
                // Move right to increase 'eggBreak' and decrease 'eggNotBreak'.
                low = mid + 1;
            }
        }
        
        // Save and return the minimum moves found for this state.
        return dp[k][n] = minMoves;
    }

    public int superEggDrop(int k, int n) {
        // DP table initialized with -1 (uncomputed)
        int[][] dp = new int[k + 1][n + 1];
        for (int i = 0; i < k + 1; i++) {
            Arrays.fill(dp[i], -1);
        }
        return helper(k, n, dp);
    }
}
/*
    📊 Complexity Analysis
    * Time Complexity: O(k * n * log n)
        * We have O(k * n) states to compute (size of the DP table).
        * For each state, we perform a binary search over 'n' possible floors,
        * which takes O(log n) time.
    
    * Space Complexity: O(k * n)
        * This is the space required for the `dp` memoization table.
        * (The recursion stack depth is at most O(k+n), but O(k*n) dominates).
    */
````
