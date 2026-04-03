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
        if (nums.length == 0) return 0;

        int i = 0; // pointer for unique elements

        for (int j = 1; j < nums.length; j++) {
            if (nums[j] != nums[i]) {
                i++;
                nums[i] = nums[j];
            }
        }

        return i + 1;
    }
}
````
