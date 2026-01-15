# search in a rotated sorted array I
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 33 - Search in Rotated Sorted Array
 * ----------------------------------------------------------------------
 * There is an integer array nums sorted in ascending order (with distinct values).
 * Prior to being passed to your function, nums is possibly rotated at an unknown 
 * pivot index k (1 <= k < nums.length).
 * * Given the array nums after the possible rotation and an integer target, return 
 * the index of target if it is in nums, or -1 if it is not in nums.
 * * * You must write an algorithm with O(log n) runtime complexity.
 * * Example 1: nums = [4,5,6,7,0,1,2], target = 0 -> Output: 4
 * * Example 2: nums = [4,5,6,7,0,1,2], target = 3 -> Output: -1
 * ----------------------------------------------------------------------
 */

class Solution {
    public int search(int[] nums, int target) {
        int n = nums.length;
        int low = 0;
        int high = n - 1;

        // Standard Binary Search loop structure
        while (low <= high) {
            int mid = low + (high - low) / 2;

            // Check if we found the target immediately
            if (nums[mid] == target) {
                return mid;
            }

            // Identify which half is sorted. 
            // If nums[low] <= nums[mid], the LEFT half [low...mid] is sorted.
            else if (nums[low] <= nums[mid]) {
                // Now we check if the target lies within this sorted left half.
                // We check if target is between low and mid (inclusive).
                if (nums[low] <= target && target <= nums[mid]) {
                    // Target is in the left sorted portion, eliminate right.
                    high = mid - 1;
                } else {
                    // Target is NOT in the left sorted portion, so it must be 
                    // in the right unsorted portion.
                    low = mid + 1;
                }
            }
            // Otherwise, the RIGHT half [mid...high] must be sorted.
            else {
                // We check if the target lies within this sorted right half.
                // We check if target is between mid and high (inclusive).
                if (nums[mid] <= target && target <= nums[high]) {
                    // Target is in the right sorted portion, eliminate left.
                    low = mid + 1;
                } else {
                    // Target is NOT in the right sorted portion, so it must be
                    // in the left unsorted portion.
                    high = mid - 1;
                }
            }
        }
        
        // Target was not found
        return -1;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(log N)
 * - Although the array is rotated, we still eliminate half of the array 
 * in every step by determining which side is sorted and if the target lies there.
 * * Space Complexity: O(1)
 * - We use constant extra space for variables (low, high, mid).
 * ----------------------------------------------------------------------
 */
````