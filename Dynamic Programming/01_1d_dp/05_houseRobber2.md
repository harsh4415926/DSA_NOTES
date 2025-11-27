# house robber 2(leetcode)
## Problem Statement
This code solves the "House Robber II" problem 🏡. The houses are arranged in a circle, which means the first and last houses are now adjacent. The goal remains the same: find the maximum amount of money you can rob without robbing two adjacent houses.
## directly space optimised code using the code from house robber 1
````java
class Solution {
    /**
     * This is your standard, space-optimized House Robber I solver.
     * It efficiently finds the max loot in a LINEAR array.
     */
    public int robbb(int[] nums) {
        int n = nums.length;
        if (n == 0) return 0;
        if (n == 1) return nums[0];

        int prev1 = 0; // Represents max loot up to i-2
        int prev2 = nums[0]; // Represents max loot up to i-1
        
        for (int i = 1; i < n; i++) {
            int pick = prev1 + nums[i];
            int nonPick = prev2;
            int curi = Math.max(pick, nonPick);
            prev1 = prev2;
            prev2 = curi;
        }
        return prev2;
    }

    /**
     * This is the main function for the circular (House Robber II) problem.
     */
    public int rob(int[] nums) {
        int n = nums.length;
        if (n == 0) return 0;
        if (n == 1) return nums[0]; // For one house, there's no circle.

        // SCENARIO 1: Exclude the first house.
        // Create a new array 'l1' containing elements from nums[1] to nums[n-1].
        int[] l1 = new int[n - 1];

        // SCENARIO 2: Exclude the last house.
        // Create a new array 'l2' containing elements from nums[0] to nums[n-2].
        int[] l2 = new int[n - 1];
        
        // Populate the two temporary arrays.
        int k = 0, j = 0;
        for (int i = 0; i < n; i++) {
            if (i != 0) l1[k++] = nums[i]; // l1 gets everything but the first element.
            if (i != n - 1) l2[j++] = nums[i]; // l2 gets everything but the last element.
        }
        
        // Solve the linear problem for both scenarios and return the maximum result.
        return Math.max(robbb(l1), robbb(l2));
    }
}
````
