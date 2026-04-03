# Problem: Move Zeroes
Given an integer array nums, move all 0's to the end of the array while preserving the relative order of the non-zero elements. This operation must be performed in-place without making a copy of the array.

Example: Input: nums = [0, 1, 0, 3, 12] Output: [1, 3, 12, 0, 0]

---
````java
class Solution {
    public void moveZeroes(int[] nums) {
        int i = 0; // position for next non-zero

        for (int j = 0; j < nums.length; j++) {
            if (nums[j] != 0) {
                int temp = nums[i];
                nums[i] = nums[j];
                nums[j] = temp;
                i++;
            }
        }
    }
}
````