# burst ballons(leetcode)
##  Burst Balloons vs. MCM

### Why the Standard MCM "Split at k" Logic Fails Here

In a typical MCM problem for a range `[i, j]`, we find a split point `k` (e.g., `(A[i...k]) * (A[k+1...j])`). The key is that the two subproblems `(i, k)` and `(k+1, j)` are **completely independent**.

Let's try that here. What if we try to burst the **FIRST** balloon `k` in the range `[i, j]`?

1.  **Cost:** `nums[i-1] * nums[k] * nums[j+1]` (Using padded 1s as boundaries)
2.  **Subproblems:** We are left with two ranges: `[i, k-1]` and `[k+1, j]`.
3.  **The Problem:** These subproblems are **NOT independent**. When `k` is burst, `k-1` and `k+1` become adjacent. Now, the score you get from bursting `k-1` depends on `k+1` (which is in the *other* subproblem). This "linking" of subproblems breaks the DP optimal substructure.

### The Correct Strategy: Bursting "LAST"

Since picking the *first* balloon to burst creates dependent subproblems, we reverse the logic: **What if we pick the *LAST* balloon `k` to burst in the range `[i, j]`?**

1.  **Event:** We iterate `idx` from `i` to `j`, assuming `arr[idx]` is the **last** balloon to be burst in this range.
2.  **State at that Event:** If `arr[idx]` is the last to burst, it means every other balloon in `[i, j]` (i.e., `[i, idx-1]` and `[idx+1, j]`) has *already been burst*.
3.  **Cost:** When `arr[idx]` is finally burst, what are its neighbors? They *must* be the original boundaries of the range `[i, j]`, which are `arr[i-1]` and `arr[j+1]`.
    * So, the cost of this *final* burst is: `arr[i-1] * arr[idx] * arr[j+1]`.
4.  **Subproblems:** Before we could burst `idx`, we *must* have already solved two independent subproblems:
    * **Left Subproblem:** Bursting all balloons in `[i, idx-1]`. This is solved by `helper(i, idx-1)`. The boundaries for *this* subproblem are `arr[i-1]` and `arr[idx]`.
    * **Right Subproblem:** Bursting all balloons in `[idx+1, j]`. This is solved by `helper(idx+1, j)`. The boundaries for *this* subproblem are `arr[idx]` and `arr[j+1]`.

The brilliance of this is that the subproblems `helper(i, idx-1)` and `helper(idx+1, j)` are **truly independent**. The balloon `arr[idx]` acts as a "wall" or boundary for both, and they don't interfere with each other.

## memoization
````java
// This solution uses Dynamic Programming with Memoization.
// The core idea is based on the "Matrix Chain Multiplication" pattern.
// We try to find the *last* balloon to burst in any given range [i, j].

class Solution {
    
    /**
     * Helper function to perform the recursive calculation with memoization.
     * @param i   The starting index of the subarray (from the modified 'arr')
     * @param j   The ending index of the subarray (from the modified 'arr')
     * @param arr The modified list of balloon values (with 1s at both ends)
     * @param dp  The memoization table to store results of subproblems dp[i][j]
     * @return The maximum coins obtainable from bursting balloons in the range [i, j]
     */
    public int helper(int i, int j, List<Integer> arr, int[][] dp) {
        
        // Base Case: If i > j, it means there are no balloons in this range to burst.
        // So, the coins obtained is 0.
        if (i > j) {
            return 0;
        }

        // Memoization Check: If we have already computed the result for this subproblem (i, j),
        // return the stored value to avoid redundant calculations.
        if (dp[i][j] != -1) {
            return dp[i][j];
        }

        // Initialize max coins for this range [i, j] to the minimum possible value.
        int max = Integer.MIN_VALUE;

        // Iterate through all possible balloons 'idx' in the range [i, j].
        // We assume 'idx' is the *LAST* balloon to be burst in this range.
        for (int idx = i; idx <= j; idx++) {
            
            // If arr[idx] is the *last* balloon to burst in range [i, j],
            // all balloons from [i, idx-1] and [idx+1, j] are already burst.
            // This means the neighbors of 'idx' at the time of its burst are
            // arr[i-1] (the left boundary) and arr[j+1] (the right boundary).
            
            // Calculate the total cost:
            int cost = 
                // 1. Coins from bursting balloon 'idx' last:
                arr.get(i - 1) * arr.get(idx) * arr.get(j + 1)
                
                // 2. Add max coins from the left subproblem (bursting all balloons from i to idx-1):
                + helper(i, idx - 1, arr, dp)
                
                // 3. Add max coins from the right subproblem (bursting all balloons from idx+1 to j):
                + helper(idx + 1, j, arr, dp);

            // Update the maximum cost found so far for the range [i, j].
            max = Math.max(max, cost);
        }

        // Store the computed maximum cost for range [i, j] in the DP table and return it.
        return dp[i][j] = max;
    }

    /**
     * Main function to set up the problem.
     * @param nums The original array of balloon values.
     * @return The maximum coins obtainable.
     */
    public int maxCoins(int[] nums) {
        int n = nums.length;

        // Create a new list 'arr' to store balloon values.
        List<Integer> arr = new ArrayList<>();

        // Add a virtual balloon with value 1 at the beginning.
        // This acts as a boundary.
        arr.add(1);

        // Add all the original balloon values.
        for (int i = 0; i < n; i++) {
            arr.add(nums[i]);
        }

        // Add a virtual balloon with value 1 at the end.
        // This also acts as a boundary.
        // Now, arr = [1, nums[0], nums[1], ..., nums[n-1], 1]
        arr.add(1);

        // Create a DP table. dp[i][j] will store the max coins from
        // bursting balloons in the original index range [i-1, j-1].
        // The size is (n+1)x(n+1) because our new indices 'i' and 'j'
        // in the helper function will range from 1 to n.
        int[][] dp = new int[n + 1][n + 1];

        // Initialize the DP table with -1 (or any sentinel value)
        // to indicate that the subproblems haven't been solved yet.
        for (int i = 0; i < n + 1; i++) {
            Arrays.fill(dp[i], -1);
        }

        // Call the helper function for the range of *original* balloons.
        // In the modified 'arr' list, these correspond to indices 1 to n.
        // arr[0] is the left boundary (1).
        // arr[n+1] is the right boundary (1).
        return helper(1, n, arr, dp);
    }
}
````
## tabulation
````java
class Solution {
    public int maxCoins(int[] nums) {
        int n = nums.length;
        // Pad the array with 1s at both ends to handle edge cases.
        List<Integer> arr = new ArrayList<>();
        arr.add(1);
        for (int i = 0; i < n; i++) arr.add(nums[i]);
        arr.add(1);
        
        // dp[i][j] stores max coins from bursting balloons i to j (original indices).
        int[][] dp = new int[n + 2][n + 2];

        // Iterate backwards from the end of the array.
        for (int i = n; i >= 1; i--) {
            // j starts from i and goes to the end. This iterates over all subarrays.
            for (int j = i; j <= n; j++) {
                
                int max = Integer.MIN_VALUE;
                // 'idx' represents the *last* balloon to be burst in the range [i, j].
                for (int idx = i; idx <= j; idx++) {
                    // Cost = (bursting 'idx' last) + (cost of left part) + (cost of right part).
                    int cost = arr.get(i - 1) * arr.get(idx) * arr.get(j + 1)
                               + dp[i][idx - 1]
                               + dp[idx + 1][j];
                    max = Math.max(max, cost);
                }
                dp[i][j] = max;
            }
        }
        
        // The answer is the max cost for bursting the entire original range [1...n].
        return dp[1][n];
    }

    // Time Complexity: O(N^3)
    // - Where N is the number of balloons.
    // - Three nested loops (i, j, idx) each run up to N times.

    // Space Complexity: O(N^2)
    // - For the (N+2) x (N+2) DP table.
}
````