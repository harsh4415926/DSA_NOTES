# single element in sorted array
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 540 - Single Element in a Sorted Array
 * ----------------------------------------------------------------------
 * You are given a sorted array consisting of only integers where every
 * element appears exactly twice, except for one element which appears
 * exactly once.
 * * Return the single element that appears only once.
 * * You must implement a solution with a runtime complexity of O(log n)
 * and O(1) space.
 * * Example 1: nums = [1,1,2,3,3,4,4,8,8] -> Output: 2
 * Example 2: nums = [3,3,7,7,10,11,11] -> Output: 10
 * ----------------------------------------------------------------------
 */

class Solution {
    public int singleNonDuplicate(int[] nums) {
        int n = nums.length;

        // Edge Case 1: If array has only one element, that is the answer.
        if (n == 1) return nums[0];

        // Edge Case 2: Check the first element.
        // Since it's sorted, if nums[0] != nums[1], nums[0] is the single element.
        if (nums[0] != nums[1]) return nums[0];

        // Edge Case 3: Check the last element.
        // If nums[n-1] != nums[n-2], nums[n-1] is the single element.
        if (nums[n - 1] != nums[n - 2]) return nums[n - 1];

        // Binary Search Space:
        // We trim the search space to 1...n-2 because we already handled 
        // the 0th and (n-1)th indices above. This prevents IndexOutOfBounds
        // when checking nums[mid-1] or nums[mid+1].
        int low = 1;
        int high = n - 2;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            // Check if 'mid' is the single element.
            // If it is different from both its left and right neighbors, we found it.
            if (nums[mid] != nums[mid - 1] && nums[mid] != nums[mid + 1]) {
                return nums[mid];
            }

            // LOGIC FOR ELIMINATION:
            // Left of the single element: Pairs are (Even Index, Odd Index).
            // Right of the single element: Pairs are (Odd Index, Even Index).

            // We check if we are in the "Left Side" (valid pair pattern):
            // 1. (mid is odd AND matches previous element) OR 
            // 2. (mid is even AND matches next element)
            if ((nums[mid] == nums[mid - 1] && mid % 2 != 0) ||
                    (nums[mid] == nums[mid + 1] && mid % 2 == 0)) {

                // We are on the left side, so the single element is to the right.
                low = mid + 1;
            }
            else {
                // We are on the right side (pattern disrupted), 
                // so the single element must be to the left.
                high = mid - 1;
            }
        }
        return -1; // Should technically not reach here given problem constraints
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(log N)
 * - We use Binary Search to cut the search space in half at each step.
 * - We only check index parity (odd/even) and neighbors, which is O(1).
 * * Space Complexity: O(1)
 * - We use constant extra space for variables (n, low, high, mid).
 * ----------------------------------------------------------------------
 */
````