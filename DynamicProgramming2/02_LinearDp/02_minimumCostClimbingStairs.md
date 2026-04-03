````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 746 - Min Cost Climbing Stairs
 * ----------------------------------------------------------------------
 * LOGIC (Pure Recursion):
 * 1. The goal is to reach the "top" of the stairs (index == cost.length) 
 * with the minimum total cost.
 * 2. You can choose to start your climb at index 0 or index 1. We must 
 * calculate the minimum cost from both starting points and take the 
 * overall minimum.
 * 3. At any step 'i', you pay the cost of that step (`cost[i]`), and then 
 * you have a choice: take 1 step to `i + 1`, or 2 steps to `i + 2`.
 * 4. Base Case: If you reach or step past the top of the array 
 * (`i >= cost.length`), the cost to go any further is 0 because you're done.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int minCostClimbingStairs(int[] cost) {
        // We can start at either step 0 or step 1. 
        // Return the minimum of these two parallel recursive paths.
        return Math.min(solve(0, cost), solve(1, cost));
    }

    private int solve(int i, int[] cost) {
        // Base case: We have reached or crossed the top floor. No more cost.
        if (i >= cost.length) return 0;

        // Recursive choice:
        // The cost to finish from step 'i' is the cost of standing on step 'i'
        // PLUS the minimum cost of finishing from either 1 step or 2 steps ahead.
        return cost[i] + Math.min(
            solve(i + 1, cost), // Branch 1: Take 1 step
            solve(i + 2, cost)  // Branch 2: Take 2 steps
        );
    }
}

/*
 * COMPLEXITY:
 * Time: O(2^n) - Exponential. Just like pure recursive Fibonacci, this tree 
 * branches out and recalculates the exact same overlapping subproblems 
 * repeatedly. This will cause a Time Limit Exceeded (TLE) on LeetCode.
 * Space: O(n) - Maximum depth of the recursion call stack.
 */
````