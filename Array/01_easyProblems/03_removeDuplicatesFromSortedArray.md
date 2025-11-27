# Problem: Remove Duplicates from Sorted Array
Given a sorted integer array nums, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements must be maintained. Return the number of unique elements (k).

Example:

Input: nums = [0, 0, 1, 1, 1, 2, 2, 3, 3, 4]

Output: 5, and nums is modified to [0, 1, 2, 3, 4, _, _, _, _, _]

Explanation: The function returns k = 5, and the first five elements of nums are 0, 1, 2, 3, 4.

---
````java
class Solution {
    public int removeDuplicates(int[] nums) {
        int n = nums.length;
        if (n == 0) return 0; // Handle empty array

        // 'idx' is the "slow pointer" that tracks the position to place the next unique element.
        int idx = 1;
        // 'count' tracks the number of unique elements found.
        int count = 1;

        // 'i' is the "fast pointer" that iterates through the array.
        for (int i = 1; i< n; i++) {
            // If we find a new unique element...
            if (nums[i] > nums[i - 1]) {
                // ...place it at the 'idx' position.
                nums[idx++] = nums[i];
                count++;
            }
            // If nums[i] is a duplicate, the 'i' pointer moves on, but 'idx' does not.
        }
        
        // Return the total count of unique elements.
        return count;
    }

    // Time Complexity: O(N)
    // - We iterate through the array once with the 'i' pointer.

    // Space Complexity: O(1)
    // - The operation is done in-place with only a few constant variables.
}
````
