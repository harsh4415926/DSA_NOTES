````java
/*
* ----------------------------------------------------------------------
* PROBLEM: LeetCode 70 - Climbing Stairs
* ----------------------------------------------------------------------
* LOGIC (Pure Recursion):
* 1. To reach the n-th step, you must have stepped from either the
* (n-1)-th step (by taking 1 step) OR the (n-2)-th step (by taking 2 steps).
* 2. Therefore, the total ways to reach step 'n' is simply the sum of
* the ways to reach 'n-1' and 'n-2'. This perfectly mirrors the
* Fibonacci sequence.
* 3. Base cases:
* - If n == 0: You are at the ground. There is exactly 1 way to stay there.
* - If n < 0: You overshot the ground. There are 0 valid ways.
* ----------------------------------------------------------------------
*/

class Solution {
public int climbStairs(int n) {
return solve(n);
}

    private int solve(int n) {
        // Base cases
        if (n == 0) return 1; // Successfully reached the bottom
        if (n < 0) return 0;  // Invalid path

        // Recursive calls: 
        // Branch 1: Take 1 step down
        // Branch 2: Take 2 steps down
        return solve(n - 1) + solve(n - 2);
    }
}

/*
* COMPLEXITY:
* Time: O(2^n) - Exponential. Because we don't store results, this calculates
* the exact same subproblems over and over. (This will result in a Time
* Limit Exceeded (TLE) error on LeetCode for n = 45).
* Space: O(n) - Maximum depth of the recursion call stack.
  */
````