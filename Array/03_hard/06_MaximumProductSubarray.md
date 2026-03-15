# Problem: Maximum Product Subarray
## optimal
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 152 - Maximum Product Subarray
 * ----------------------------------------------------------------------
 * LOGIC (Prefix & Suffix Sweep):
 * 1. A subarray with max product must start from the very beginning
 * or very end of a segment delimited by zeros.
 * 2. Zeros break the chain, so we reset the running product to 1.
 * 3. Negatives flip signs; a single pass (prefix) isn't enough because
 * the max product might result from an even number of negatives
 * starting from the right side. Hence, we check both Prefix and Suffix.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int maxProduct(int[] nums) {
        int n = nums.length;
        int prefix = 1;
        int suffix = 1;
        int max = Integer.MIN_VALUE;

        for(int i = 0; i < n; i++) {
            // Reset logic: If previous product was 0, start fresh for new subarray
            if(prefix == 0) prefix = 1;
            if(suffix == 0) suffix = 1;

            // Update running products
            prefix *= nums[i];           // Scan from left -> right
            suffix *= nums[n - 1 - i];   // Scan from right -> left

            // Capture maximum at every step
            max = Math.max(max, Math.max(prefix, suffix));
        }
        return max;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N) (One pass, processing both ends).
 * Space: O(1) (Only scalar variables).
 */
````
