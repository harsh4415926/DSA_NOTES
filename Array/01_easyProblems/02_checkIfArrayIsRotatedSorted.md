# Problem: Check if Array Is Sorted and Rotated
Given an array nums, your task is to return true if the array was originally sorted in non-decreasing order and then rotated some number of positions (including zero rotations). Otherwise, return false.

Example:

Input: nums = [3, 4, 5, 1, 2]

Output: true

Explanation: The original array could have been [1, 2, 3, 4, 5], which was then rotated 3 positions.

---
````java
class Solution {
    public boolean check(int[] nums) {
        int n = nums.length;
        // 'count' tracks the number of "breaks" where nums[i] > nums[i+1].
        int count = 0;
        
        // A valid rotated sorted array can have at most one such "break".
        for (int i = 0; i < n - 1; i++) {
            if (nums[i] > nums[i + 1])
                count++;
        }

        // Case 1: No breaks (count == 0). The array is fully sorted.
        if (count == 0)
            return true;
        
        // Case 2: One break (count == 1). We must also check if the last
        // element is less than or equal to the first (wraps around correctly).
        else if (count == 1 && nums[n - 1] <= nums[0])
            return true;

        // Case 3: More than one break, or a single break that doesn't wrap.
        return false;
    }

    // Time Complexity: O(N)
    // - We iterate through the array once.

    // Space Complexity: O(1)
    // - We only use a few constant variables.
}
   ````