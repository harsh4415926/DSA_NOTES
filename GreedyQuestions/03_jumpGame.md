# jump game
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 55 - Jump Game
 * ----------------------------------------------------------------------
 * LOGIC (Greedy Reachability):
 * 1. Track `maxIdx`: The furthest index we can currently reach.
 * 2. Iterate through array.
 * 3. FAILURE: If current index `i` > `maxIdx`, we are stuck. Return false.
 * 4. UPDATE: `maxIdx = max(maxIdx, i + nums[i])`.
 * ----------------------------------------------------------------------
 */

class Solution {
    public boolean canJump(int[] nums) {
        int n = nums.length;
        int maxIdx = 0; // Furthest reachable index
        
        for (int i = 0; i < n; i++) {
            // If current index is beyond our max reach, we can't get here
            if (i > maxIdx) return false;
            
            // Update max reach from current position
            maxIdx = Math.max(maxIdx, i + nums[i]);
        }
        
        // Return true if we can reach (or pass) the last index
        return (maxIdx >= n - 1);
    }
}

/*
 * COMPLEXITY:
 * Time: O(N) (One pass).
 * Space: O(1) (One variable).
 */
````
