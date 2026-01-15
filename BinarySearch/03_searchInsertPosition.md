# searchInsertPosition
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 35 - Search Insert Position
 * ----------------------------------------------------------------------
 * Given a sorted array of distinct integers and a target value, return 
 * the index if the target is found. If not, return the index where it 
 * would be if it were inserted in order.
 * * Example 1: nums = [1,3,5,6], target = 5 -> Output: 2
 * Example 2: nums = [1,3,5,6], target = 2 -> Output: 1
 * Example 3: nums = [1,3,5,6], target = 7 -> Output: 4
 * ----------------------------------------------------------------------
 */

class Solution {
    public int searchInsert(int[] nums, int target) {
        int n = nums.length;
        int low = 0;
        int high = n - 1;
        
        // Initialize ans to n because if the target is greater than all elements,
        // it should be inserted at the very end of the array.
        int ans = n;

        // Standard Binary Search loop
        while (low <= high) {
            // Calculate mid to avoid potential integer overflow with (low + high)
            int mid = low + (high - low) / 2;

            if (nums[mid] > target) {
                // If current element is greater than target, this index (mid)
                // is a potential candidate for insertion.
                // We store it in 'ans' and try to find a smaller/better index 
                // on the left side.
                high = mid - 1;
                ans = mid;
            } 
            else if (nums[mid] < target) {
                // If current element is smaller than target, the insertion point
                // must be to the right. We discard the left half.
                low = mid + 1;
            } 
            else {
                // If nums[mid] == target, we found the element.
                // The index is mid, and we break immediately.
                ans = mid;
                break;
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
 * - We are using Binary Search, which divides the search space in half 
 * in every iteration. N is the number of elements in the array.
 * * Space Complexity: O(1)
 * - We only use a constant amount of extra space for variables 
 * (low, high, mid, ans). No recursion stack or extra arrays are used.
 * ----------------------------------------------------------------------
 */
````