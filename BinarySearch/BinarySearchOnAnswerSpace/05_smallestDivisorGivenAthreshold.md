# smallest divisor given a threshold

````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 1283 - Find the Smallest Divisor Given a Threshold
 * ----------------------------------------------------------------------
 * LOGIC (Binary Search on Answer):
 * 1. Concept: As the divisor INCREASES, the sum DECREASES. 
 * This monotonicity allows us to use Binary Search.
 * 2. Search Space: [1, max(nums)]. A divisor larger than max(nums) 
 * always yields a sum equal to nums.length (since ceil(x/large) = 1), 
 * so we don't need to search beyond max(nums).
 * 3. Strategy: 
 * - If a divisor 'mid' is valid (sum <= threshold), it's a potential 
 * answer. We then try to find an even smaller divisor (search left).
 * - If it's invalid (sum > threshold), the divisor is too small, 
 * forcing us to search for a larger divisor (search right).
 * ----------------------------------------------------------------------
 */

class Solution {

    // Helper function to check if a divisor is valid
    public boolean isValid(int[] nums, int divisor, int threshold) {
        int sum = 0;

        for (int num : nums) {
            // ceil(num / divisor) without using double
            sum += (num + divisor - 1) / divisor;

            if (sum > threshold) {
                return false;   // early stop (optimization)
            }
        }

        return sum <= threshold;
    }

    public int smallestDivisor(int[] nums, int threshold) {

        int low = 1;
        int high = 0;

        // Find max element for upper bound
        for (int num : nums) {
            high = Math.max(high, num);
        }

        int ans = high;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (isValid(nums, mid, threshold)) {
                ans = mid;        // possible answer
                high = mid - 1;   // try smaller divisor
            } else {
                low = mid + 1;    // need bigger divisor
            }
        }

        return ans;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N * log(M)) where N is nums.length and M is the maximum element in nums.
 * Space: O(1) tracking scalar variables.
 */
````