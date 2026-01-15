# find peak element 
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 162 - Find Peak Element
 * ----------------------------------------------------------------------
 * A peak element is an element that is strictly greater than its neighbors.
 * * Given a 0-indexed integer array nums, find a peak element, and return 
 * its index. If the array contains multiple peaks, return the index to 
 * any of the peaks.
 * * You may imagine that nums[-1] = nums[n] = -∞ (negative infinity).
 * * You must write an algorithm that runs in O(log n) time.
 * * Example 1: nums = [1,2,3,1] -> Output: 2 (3 is a peak)
 * Example 2: nums = [1,2,1,3,5,6,4] -> Output: 1 or 5
 * ----------------------------------------------------------------------
 */

class Solution {
    public int findPeakElement(int[] nums) {
        int n = nums.length;

        // Edge Case 1: Single element array.
        // By definition (neighbors are -∞), the single element is a peak.
        if (n == 1) return 0;

        // Edge Case 2: Check first element.
        // If nums[0] > nums[1], since nums[-1] is -∞, nums[0] is a peak.
        if (nums[0] > nums[1]) return 0;

        // Edge Case 3: Check last element.
        // If nums[n-1] > nums[n-2], since nums[n] is -∞, nums[n-1] is a peak.
        if (nums[n - 1] > nums[n - 2]) return n - 1;

        // Standard Binary Search Setup:
        // We start from 1 and end at n-2 because we already checked indices 0 and n-1.
        // This ensures mid-1 and mid+1 never go out of bounds.
        int low = 1;
        int high = n - 2;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            // Check if 'mid' is a peak
            // It must be strictly greater than both its left and right neighbors.
            if (nums[mid] > nums[mid - 1] && nums[mid] > nums[mid + 1]) {
                return mid;
            }
            
            // Deciding direction:
            // If we are on an increasing slope (nums[mid] > nums[mid-1]), 
            // the peak must be to the right.
            // Why? Because if it keeps increasing, the last element is the peak.
            // If it drops at some point, that drop point is the peak.
            else if (nums[mid] > nums[mid - 1]) {
                low = mid + 1;
            } 
            // If we are on a decreasing slope (or a valley), the peak is to the left.
            else {
                high = mid - 1;
            }
        }
        return -1; // Should not be reached given problem constraints
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(log N)
 * - We reduce the search space by half in each iteration based on the 
 * slope of the curve at 'mid'.
 * * Space Complexity: O(1)
 * - We only use constant extra space for variables (low, high, mid).
 * ----------------------------------------------------------------------
 */
````