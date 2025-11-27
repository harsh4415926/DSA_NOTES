# Problem: Minimum Cost to Cut a Stick
You are given a wooden stick of length n and an array cuts containing positions where you must make cuts. The cost of making a cut is equal to the length of the stick segment you are cutting. Your goal is to find the minimum total cost to perform all the cuts.

Example:

Input: n = 7, cuts = [1, 3, 4, 5]

Stick: [0, 1, 3, 4, 5, 7]

Optimal Cut Order: Cut at 4, then 3, then 5, then 1.

Cost: 7 (for cut at 4) + 4 (for cut at 3) + 3 (for cut at 5) + 2 (for cut at 1) = 16.

Output: 16

---
## memoization
````java
class Solution {
    // helper(i, j, ...) finds the min cost to make all cuts between l.get(i) and l.get(j).
    public int helper(int i, int j, List<Integer> l, int[][] dp) {
        // Base case: If there are no cuts to make in this segment (i > j).
        if (i > j) return 0;
        
        // Return the cached result if available.
        if (dp[i][j] != -1) return dp[i][j];
        
        int min = Integer.MAX_VALUE;
        // Try making the first cut at every possible position 'k' within the current segment.
        for (int k = i; k <= j; k++) {
            // Cost = (cost of the current cut) + (min cost for the left part) + (min cost for the right part).
            // Cost of the current cut is the length of the current stick segment.
            int cost = (l.get(j + 1) - l.get(i - 1)) + helper(i, k - 1, l, dp) + helper(k + 1, j, l, dp);
            min = Math.min(min, cost);
        }
        
        // Cache and return the minimum cost for this subproblem.
        return dp[i][j] = min;
    }

    public int minCost(int n, int[] cuts) {
        // Sort the cuts to handle them in a structured way.
        Arrays.sort(cuts);
        
        // Create a new list with the boundaries (0 and n) to define stick segments.
        List<Integer> l = new ArrayList<>();
        l.add(0);
        for (int i = 0; i < cuts.length; i++) l.add(cuts[i]);
        l.add(n);
        
        int c = cuts.length;
        // dp[i][j] will store the min cost for cuts from index i to j in the new list.
        int[][] dp = new int[c + 1][c + 1];
        for (int i = 0; i < c + 1; i++) Arrays.fill(dp[i], -1);
        
        // Find the min cost for all cuts, which are from index 1 to c in the list.
        return helper(1, c, l, dp);
    }

    // Time Complexity: O(C^3)
    // - Where C is the number of cuts.
    // - There are O(C^2) states, and the loop for 'k' runs O(C) times for each state.

    // Space Complexity: O(C^2)
    // - For the DP table and the recursion stack.
}
````
## tabulation
````java
class Solution {
    public int minCost(int n, int[] cuts) {
        Arrays.sort(cuts);
        List < Integer > l = new ArrayList < > ();
        l.add(0);
        for (int i = 0; i < cuts.length; i++) l.add(cuts[i]);
        l.add(n);


        int c = cuts.length;
        int[][] dp = new int[c + 2][c + 2];
        //base case
        for (int i = 0; i < c + 1; i++) {
            for (int j = 0; j < c + 1; j++) {
                if (i > j) dp[i][j] = 0;
            }
        }

        for (int i = c; i >= 1; i--) {
            for (int j = i; j < c + 1; j++) {
                int min = Integer.MAX_VALUE;
                for (int k = i; k <= j; k++) {
                    int cost = (l.get(j + 1) - l.get(i - 1)) + dp[i][k - 1] + dp[k + 1][j];
                    min = Math.min(min, cost);
                }
                dp[i][j] = min;
            }
        }
        return dp[1][c];
    }
}
````
