# jump Game 2
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 45 - Jump Game II
 * ----------------------------------------------------------------------
 * LOGIC (Greedy BFS / Levels):
 * 1. Concept: Process array in "ranges" (levels).
 * - Jump 1 range: [start, currEnd]
 * - Jump 2 range: [currEnd + 1, farthest reachable from Jump 1]
 * 2. Scan through current range, tracking 'farthest' potential reach.
 * 3. When 'i' hits 'currEnd', we force a jump to the next range.
 * - Update currEnd = farthest.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int jump(int[] nums) {
        int jumps = 0;
        int currEnd = 0;   // Boundary of current jump count
        int farthest = 0;  // Max reach available for NEXT jump

        // Loop stops BEFORE last index. 
        // Why? We only jump if we are NOT at the end yet.
        for (int i = 0; i < nums.length - 1; i++) {
            
            // Greedily find farthest reach within current jump range
            farthest = Math.max(farthest, i + nums[i]);

            // Reached the limit of current jump?
            if (i == currEnd) {
                jumps++;            // Must take a jump now
                currEnd = farthest; // Set boundary for next jump
            }
        }
        return jumps;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N) (Single pass).
 * Space: O(1) (Three variables).
 */
````