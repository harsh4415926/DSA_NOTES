# Problem: Maximum Product Subarray
You are given an integer array nums. Your task is to find the contiguous subarray (containing at least one number) that has the largest product and return that product. The array can contain positive, negative, and zero values.

Example: Input: nums = [2, 3, -2, 4] Output: 6 Explanation: The subarray [2, 3] has the largest product of 6.

---
## optimal
````java
class Solution {
    /**
     * Finds the maximum product of a contiguous subarray.
     * This approach cleverly uses two "passes" in one loop:
     * 1. A 'prefix' product, accumulating from the start.
     * 2. A 'suffix' product, accumulating from the end.
     * This handles cases with an even or odd number of negative numbers.
     * @param nums The input array.
     * @return The maximum product found.
     */
    public int maxProduct(int[] nums) {
        int n = nums.length;
        int prefix = 1;  // Stores the product from left-to-right
        int suffix = 1;  // Stores the product from right-to-left
        int max = Integer.MIN_VALUE; // Stores the max product found so far

        for (int i = 0; i < n; i++) {
            // If prefix or suffix becomes 0, reset it to 1.
            // This effectively treats 0 as a "break" in the subarray.
            if (prefix == 0) prefix = 1;
            if (suffix == 0) suffix = 1;

            // Update prefix product
            prefix *= nums[i];
            
            // Update suffix product (moving from right to left)
            suffix *= nums[n - 1 - i];

            // Update the maximum product
            max = Math.max(max, Math.max(prefix, suffix));
        }
        return max;

        // Time Complexity: O(N)
        // - We iterate through the array once.
        
        // Space Complexity: O(1)
        // - We only use a few constant-space variables.
    }
}
````
