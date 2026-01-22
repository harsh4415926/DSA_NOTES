# search in rotated sorted array II
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 81 - Search in Rotated Sorted Array II
 * ----------------------------------------------------------------------
 * This is similar to "Search in Rotated Sorted Array" (LeetCode 33), 
 * but the array MAY contain duplicates.
 * * The array is sorted in non-decreasing order and then rotated.
 * * Given a target integer, return true if it exists in the array, 
 * otherwise return false.
 * * The presence of duplicates creates ambiguity in determining which 
 * side is sorted, affecting the worst-case time complexity.
 * * Example 1: nums = [2,5,6,0,0,1,2], target = 0 -> Output: true
 * Example 2: nums = [2,5,6,0,0,1,2], target = 3 -> Output: false
 * ----------------------------------------------------------------------
 */

class Solution {
    public boolean search(int[] nums, int target) {
        int n = nums.length;
        int low = 0;
        int high = n - 1;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (nums[mid] == target) {
                return true;
            }

            // CRITICAL EDGE CASE: Handling Duplicates
            // If low, mid, and high pointers all have the same value, 
            // we cannot determine which half is sorted.
            // Example: [1, 0, 1, 1, 1] vs [1, 1, 1, 0, 1]
            // In both, nums[low] == nums[mid] == nums[high] == 1.
            // Solution: Shrink the search space from both ends linearly.
            if (nums[low] == nums[mid] && nums[mid] == nums[high]) {
                low++;
                high--;
                continue; // Skip the rest of the loop and re-calculate mid
            }

            // If left side is sorted (standard binary search logic)
            // Note: because we handled the triple equality above, 
            // the case nums[low] == nums[mid] here implies the left side is sorted validly.
            if (nums[low] <= nums[mid]) {
                // Check if target is in the left sorted range
                if (nums[low] <= target && target <= nums[mid]) {
                    high = mid - 1;
                } else {
                    low = mid + 1;
                }
            }
            // Otherwise, the right side is sorted
            else {
                // Check if target is in the right sorted range
                if (nums[mid] <= target && target <= nums[high]) {
                    low = mid + 1;
                } else {
                    high = mid - 1;
                }
            }
        }
        return false;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: 
 * - Average Case: O(log N) - Similar to standard Binary Search.
 * - Worst Case: O(N) - This happens if the array consists mainly of 
 * duplicates (e.g., [1, 1, 1, 1]). The algorithm performs linear 
 * shrinking (low++, high--) and checks every element.
 * * Space Complexity: O(1)
 * - We use constant extra space for variables.
 * ----------------------------------------------------------------------
 */
````
