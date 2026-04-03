````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 1186 - Maximum Subarray Sum with One Deletion
 * ----------------------------------------------------------------------
 * LOGIC (State Machine DP):
 * 1. 'keep' tracks the maximum subarray sum ending at 'i' with NO deletions.
 * 2. 'del' tracks the maximum subarray sum ending at 'i' with EXACTLY ONE deletion.
 * 3. Transitions for the current element 'arr[i]':
 * - For 'del':
 * Option A: We already deleted an element earlier, so we add arr[i] to the previous 'del'.
 * Option B: We use our 1 allowed deletion right NOW on arr[i], so the sum is just the previous 'keep'.
 * - For 'keep': Standard Kadane's. Either add arr[i] to previous 'keep', or start fresh.
 * 4. CRITICAL: 'del' must be updated before 'keep' so it correctly uses the 'keep' value from step i-1.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int maximumSum(int[] arr) {
        int n = arr.length;

        // Base cases initialized to the first element
        int keep = arr[0];
        int del = 0;
        int ans = arr[0];

        for(int i = 1; i < n; i++) {

            // Update 'del' state FIRST.
            // Option 1: Deletion happened in the past (del + arr[i])
            // Option 2: Deletion happens right now (previous 'keep')
            del = Math.max(del + arr[i], keep);

            // Update 'keep' state (Standard Kadane's)
            // Either extend the contiguous sum, or start a new one here
            keep = Math.max(keep + arr[i], arr[i]);

            // The global maximum could come from either state
            ans = Math.max(ans, Math.max(keep, del));
        }

        return ans;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N) (A single linear pass).
 * Space: O(1) (Only tracking three variables, which is optimal).
 */

````