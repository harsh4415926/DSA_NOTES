# first and last occurence of element 
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 34 - Find First and Last Position of Element in Sorted Array
 * ----------------------------------------------------------------------
 * Given an array of integers 'nums' sorted in non-decreasing order, 
 * find the starting and ending position of a given 'target' value.
 * * If the target is not found in the array, return [-1, -1].
 * * You must write an algorithm with O(log n) runtime complexity.
 * * Example 1: nums = [5,7,7,8,8,10], target = 8 -> Output: [3,4]
 * Example 2: nums = [5,7,7,8,8,10], target = 6 -> Output: [-1,-1]
 * ----------------------------------------------------------------------
 */

class Solution {

    // Helper method to find the Lower Bound
    // Lower Bound: First index where nums[index] >= target
    public int lowerbound(int[] nums, int target) {
        int n = nums.length;
        int low = 0;
        int high = n - 1;
        int ans = n; // Default answer is array length (if all elements < target)

        while (low <= high) {
            int mid = low + (high - low) / 2;
            
            // If current element is >= target, it could be the lower bound.
            // We store it and try to find a smaller valid index on the left.
            if (nums[mid] >= target) {
                ans = mid;
                high = mid - 1;
            } 
            else {
                // If nums[mid] < target, the answer must be on the right.
                low = mid + 1;
            }
        }
        return ans;
    }

    // Helper method to find the Upper Bound
    // Upper Bound: First index where nums[index] > target
    public int upperbound(int[] nums, int target) {
        int n = nums.length;
        int low = 0;
        int high = n - 1;
        int ans = n; // Default answer is array length

        while (low <= high) {
            int mid = low + (high - low) / 2;
            
            // If current element is > target, it qualifies as an upper bound.
            // We store it and try to find a smaller index on the left (closer to target).
            if (nums[mid] > target) {
                ans = mid;
                high = mid - 1;
            } 
            else {
                // If nums[mid] <= target, we need to go right to find a value > target.
                low = mid + 1;
            }
        }
        return ans;
    }

    public int[] searchRange(int[] nums, int target) {
        // Step 1: Find the first occurrence (Lower Bound)
        int lb = lowerbound(nums, target);
        
        // Validation:
        // 1. If lb is out of bounds (lb == n), target is greater than all elements.
        // 2. If nums[lb] is not the target, then the target doesn't exist in the array.
        if (lb >= nums.length || nums[lb] != target) {
            return new int[]{-1, -1};
        }

        // Step 2: Find the Upper Bound (index of the first element strictly > target)
        int ub = upperbound(nums, target);

        // The Upper Bound returns the index *after* the last occurrence of target.
        // Therefore, the last occurrence is at index (ub - 1).
        return new int[]{lb, ub - 1};
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(log N)
 * - We perform two separate Binary Search operations (one for lower bound, 
 * one for upper bound).
 * - Complexity is 2 * O(log N), which simplifies to O(log N).
 * * Space Complexity: O(1)
 * - We use a constant amount of extra space for pointers (low, high, mid).
 * ----------------------------------------------------------------------
 */
````