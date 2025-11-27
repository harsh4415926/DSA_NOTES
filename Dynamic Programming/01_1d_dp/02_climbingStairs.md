# climbing stairs(leetcode)
same recursive function as fibonacci
## problem 
You are climbing a staircase with n steps. Each time, you can climb either 1 or 2 steps. Find how many distinct ways you can reach the top.

## memoization
````java
class Solution {
    // Helper function to calculate the number of ways to climb 'n' stairs
    public int helper(int n, int[] dp) {
        // Base case: if there are 1 or 2 stairs, the number of ways is n itself
        if(n <= 2) return n;
        
        // If we already calculated the result for 'n', return it
        if(dp[n] != -1) return dp[n];
        
        // Otherwise, calculate the result recursively and store it in dp array
        dp[n] = helper(n - 1, dp) + helper(n - 2, dp);
        
        // Return the calculated result
        return dp[n];
    }

    // Main function to calculate the number of distinct ways to climb 'n' stairs
    public int climbStairs(int n) {
        // Create dp array to store intermediate results, initialized with -1
        int[] dp = new int[n + 1];
        Arrays.fill(dp, -1);
        
        // Call the helper function to compute the result
        return helper(n, dp);
    }
}

// Time Complexity: O(n)
// - Every stair count from 1 to n is calculated at most once and stored in the dp array.

// Space Complexity: O(n)
// - We use a dp array of size n+1 to store intermediate results.
// - Additionally, the recursion call stack can go as deep as n, so O(n) space is used for recursion.
````
