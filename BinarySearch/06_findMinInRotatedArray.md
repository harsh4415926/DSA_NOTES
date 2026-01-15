# find minimum in rotated sorted Array
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 153 - Find Minimum in Rotated Sorted Array
 * ----------------------------------------------------------------------
 * Suppose an array of length n sorted in ascending order is rotated 
 * between 1 and n times. 
 * * Notice that rotating an array [a[0], a[1], a[2], ..., a[n-1]] 1 time 
 * results in the array [a[n-1], a[0], a[1], a[2], ..., a[n-2]].
 * * Given the sorted rotated array nums of unique elements, return the 
 * minimum element of this array.
 * * You must write an algorithm that runs in O(log n) time.
 * * Example 1: nums = [3,4,5,1,2] -> Output: 1
 * Example 2: nums = [4,5,6,7,0,1,2] -> Output: 0
 * Example 3: nums = [11,13,15,17] -> Output: 11
 * ----------------------------------------------------------------------
 */

class Solution {
    public int findMin(int[] nums) {
        int n = nums.length;
        int low = 0;
        int high = n - 1;
        int ans = Integer.MAX_VALUE; // Initialize with max possible value

        while (low <= high) {
            int mid = low + (high - low) / 2;

            // Strategy: Identify the sorted half.
            // The minimum element always lies in the unsorted half, 
            // OR it is the first element of the sorted half (if the whole range is sorted).

            // Check if the left half [low...mid] is sorted
            if (nums[low] <= nums[mid]) {
                // If left half is sorted, the smallest element in THIS half is nums[low].
                // We update 'ans' with nums[low] because it might be the global minimum.
                ans = Math.min(ans, nums[low]);
                
                // Since the left half is sorted and we have already accounted for 
                // its minimum (nums[low]), the only place a smaller element could exist 
                // is in the right half (if the pivot is there). So we eliminate the left.
                low = mid + 1;
            } 
            // Otherwise, the right half [mid...high] must be sorted
            else {
                // If right half is sorted, the smallest element in THIS half is nums[mid].
                // We update 'ans' with nums[mid].
                ans = Math.min(ans, nums[mid]);
                
                // Since the right half is sorted, the values increase after mid.
                // We have already recorded nums[mid], so we search the left half 
                // to see if an even smaller value exists (the pivot).
                high = mid - 1;
            }
        }
        return ans;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(log N)
 * - We are using Binary Search. In each step, we eliminate half of the 
 * search space by determining which side is sorted.
 * * Space Complexity: O(1)
 * - We only use constant extra space for variables (low, high, mid, ans).
 * ----------------------------------------------------------------------
 */
````
