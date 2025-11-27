# House Robber(leetcode)
## problem
This code solves the classic "House Robber" problem 💰. You're given an array of numbers representing money in a line of houses. The goal is to find the maximum amount of money you can steal, with the constraint that you cannot rob two adjacent houses, as that would trigger an alarm.

## memoization
````java
import java.util.Arrays;

class Solution {
    /**
     * Recursive helper with memoization.
     * Calculates the maximum sum possible up to index 'idx'.
     */
    public int maxSum(int idx, int[] nums, int[] dp) {
        // Base case: If at the first house (idx=0), the only option is to rob it.
        if (idx == 0) return nums[idx];

        // Base case: If the index is negative, there's nothing to add, so sum is 0.
        if (idx < 0) return 0;

        // Memoization: If we've already calculated the max sum for this index, return it.
        if (dp[idx] != -1) return dp[idx];

        // --- Two choices for the current house 'idx' ---

        // Option 1: "Pick" this house.
        // Add its value (nums[idx]) and the max sum from the house before the previous one (idx-2).
        int pick = nums[idx] + maxSum(idx - 2, nums, dp);

        // Option 2: "Don't Pick" this house.
        // The value is 0, and we take the max sum from the immediately preceding house (idx-1).
        int nonPick = 0 + maxSum(idx - 1, nums, dp);

        // Store the maximum of the two choices in our dp array and return it.
        return dp[idx] = Math.max(pick, nonPick);
    }

    /**
     * Main function to set up the DP array and start the process.
     */
    public int rob(int[] nums) {
        int n = nums.length;
        
        // dp[i] will store the max money that can be robbed up to house i.
        int[] dp = new int[n];

        // Initialize dp array with -1 to mark all states as not yet computed.
        Arrays.fill(dp, -1);

        // Start the recursion from the last house (n-1).
        return maxSum(n - 1, nums, dp);
    }

    // --- Complexity Analysis ---
    // Time Complexity: O(N)
    // Each state (from 0 to n-1) is computed only once due to memoization.

    // Space Complexity: O(N)
    // O(N) for the dp array and O(N) for the recursion call stack.
}
````
## tabulation
````java
import java.util.Arrays;

class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if (n == 1) return nums[0];
        
        // dp[i] stores the max money robbed up to house 'i'.
        int[] dp = new int[n];
        
        // Base case: for the first house.
        dp[0] = nums[0];
        
        int pick = 0;
        int nonPick = 0;
        
        // Iterate from the second house.
        for (int i = 1; i < n; i++) {
            
            // Option 1: Rob house 'i'.
            // Add current house's value to max money from two houses before.
            if (i - 2 >= 0)
                pick = nums[i] + dp[i-2];
            // For i=1, you can only pick the current house's value.
            else
                pick = nums[i];
                
            // Option 2: Don't rob house 'i'.
            // Max money is the same as the max up to the previous house.
            nonPick = dp[i-1];
            
            // Store the better of the two options.
            dp[i] = Math.max(pick, nonPick);
        }
        
        // The answer is the max money that can be robbed considering all houses.
        return dp[n - 1];
    }
    
    // --- Complexity Analysis ---
    // Time Complexity: O(N)
    // We iterate through the array once to fill the dp table.
    
    // Space Complexity: O(N)
    // We use an auxiliary dp array of size N to store intermediate results.
}
````
## space optimization
````java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        
        // Note: This code will fail for n=0. An initial check 'if (n==0) return 0;' would fix it.
        
        // Represents the max money robbed up to house i-2.
        int prev1 = 0;
        
        // Represents the max money robbed up to house i-1. Initialized for the first house.
        int prev2 = nums[0];
        
        // Iterate from the second house.
        for (int i = 1; i < n; i++) {
            // Option 1: Rob house 'i' + loot from two houses ago (prev1).
            int pick = prev1 + nums[i];
            
            // Option 2: Skip house 'i' and take the loot from the previous house (prev2).
            int nonPick = 0 + prev2;
            
            // Current max is the better of the two options.
            int curi = Math.max(pick, nonPick);
            
            // Update pointers for the next iteration (slide the window).
            prev1 = prev2;
            prev2 = curi;
        }
        
        // After the loop, prev2 holds the max money for the entire row of houses.
        return prev2;
    }
    
    // --- Complexity Analysis ---
    // Time Complexity: O(N)
    // A single loop runs through the array once.
    
    // Space Complexity: O(1)
    // Only a constant number of variables are used, regardless of the input size.
}
````
