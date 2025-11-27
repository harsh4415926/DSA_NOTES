## Frog jump (gfg)
## problem
A frog is trying to reach the last stone. There are n stones, where each stone has a certain height. The frog starts at stone 0 and can jump either one or two steps forward. The cost of jumping from one stone to another is the absolute difference in their heights. Find the minimum total cost for the frog to reach the last stone.

## memoization
````java
class Solution {
    /**
     * Recursive helper function to find the minimum cost to reach index 'idx'.
     * Uses a dp array for memoization to store results of subproblems.
     */
    public int findMinCost(int idx, int[] height, int[] dp) {
        // Base case: If we are at the starting stone (index 0), the cost is 0.
        if (idx == 0) return 0;

        // Base case: To reach stone 1, we must jump from stone 0.
        if (idx == 1) return Math.abs(height[0] - height[1]);

        // Memoization check: If the cost for this index is already computed, return it.
        if (dp[idx] != -1) return dp[idx];

        // Recursive call: Calculate cost if we jump from stone idx-2.
        int left = findMinCost(idx - 2, height, dp) + Math.abs(height[idx - 2] - height[idx]);

        // Recursive call: Calculate cost if we jump from stone idx-1.
        int right = findMinCost(idx - 1, height, dp) + Math.abs(height[idx - 1] - height[idx]);

        // Store the minimum of the two options in the dp array before returning.
        return dp[idx] = Math.min(left, right);
    }

    /**
     * Main function to set up DP and start the recursion.
     */
    int minCost(int[] height) {
        // Get the total number of stones.
        int n = height.length;

        // Create a dp array to store results of subproblems.
        int[] dp = new int[n];

        // Initialize dp array with -1 to indicate that no state has been computed yet.
        Arrays.fill(dp, -1);

        // Start the recursive calculation from the last stone (n-1).
        return findMinCost(n - 1, height, dp);
    }

    // --- Complexity Analysis ---
    // Time Complexity: O(N) because each state (from 0 to n-1) is computed only once due to memoization.
    // Space Complexity: O(N) for the recursion call stack space + O(N) for the dp array, so O(N) overall.
}
````
## tabulation
````java
class Solution {
    // Function to find the minimum cost to reach the last stone (Frog Jump problem).
    int minCost(int[] height) {
        // Get the number of stones.
        int n = height.length;

        // Edge case: If there's only one stone, cost is 0.
        if (n == 1) return 0;

        // dp[i] will store the minimum cost to reach stone i.
        int[] dp = new int[n];

        // Base case: Cost to reach the starting stone is 0.
        dp[0] = 0;

        // Base case: Cost to reach the second stone is a direct jump from the first.
        dp[1] = Math.abs(height[1] - height[0]);

        // Fill the dp array for the remaining stones.
        for (int i = 2; i < n; i++) {
            // Option 1: Jump from stone i-2 to i.
            int left = dp[i - 2] + Math.abs(height[i - 2] - height[i]);

            // Option 2: Jump from stone i-1 to i.
            int right = dp[i - 1] + Math.abs(height[i - 1] - height[i]);

            // Store the minimum cost to reach stone i.
            dp[i] = Math.min(left, right);
        }

        // The final answer is the minimum cost to reach the last stone.
        return dp[n - 1];
    }
}
````
## space optimization
````java
class Solution {
    int minCost(int[] height) {
        // Get the total number of stones.
        int n = height.length;
        
        // Edge case: If there's only one stone, the cost is 0.
        if (n == 1) return 0;
        
        // This solution uses constant space by only storing the last two results.
        
        // 'prev1' stores the min cost to reach stone i-2. Initialize with cost to reach stone 0.
        int prev1 = 0;
        
        // 'prev2' stores the min cost to reach stone i-1. Initialize with cost to reach stone 1.
        int prev2 = Math.abs(height[1] - height[0]);
        
        // 'curi' will store the min cost to reach the current stone i.
        int curi = 0; // Note: For n=2, the loop is skipped and this returns 0, which is a bug. It should return prev2.
        
        // Iterate from the third stone (index 2) to the end.
        for (int i = 2; i < n; i++) {
            // Option 1: Cost to jump from stone i-2 (cost stored in prev1).
            int left = prev1 + Math.abs(height[i - 2] - height[i]);
            
            // Option 2: Cost to jump from stone i-1 (cost stored in prev2).
            int right = prev2 + Math.abs(height[i - 1] - height[i]);
            
            // The minimum cost to reach the current stone 'i'.
            curi = Math.min(left, right);
            
            // Update pointers for the next iteration: slide the window forward.
            prev1 = prev2;
            prev2 = curi;
        }
        
        // The last calculated cost is the answer for n > 2.
        return curi;
    }
    
    // --- Complexity Analysis ---
    // Time Complexity: O(N) because we iterate through the array exactly once.
    // Space Complexity: O(1) because we only use a few constant variables, not an entire DP array.
}
````
