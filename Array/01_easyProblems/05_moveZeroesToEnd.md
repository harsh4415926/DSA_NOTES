# Problem: Move Zeroes
Given an integer array nums, move all 0's to the end of the array while preserving the relative order of the non-zero elements. This operation must be performed in-place without making a copy of the array.

Example: Input: nums = [0, 1, 0, 3, 12] Output: [1, 3, 12, 0, 0]

---
````java
class Solution {
    public void swap(int[] arr,int i,int j){
        // Swaps the elements at index i and j in the array
        int temp=arr[i];
        arr[i]=arr[j];
        arr[j]=temp;
    }
    public void moveZeroes(int[] nums) {
        int n=nums.length;
        // 'idx' tracks the position where the next non-zero element should be placed
        int idx=0;
        // Iterate through the array
        for(int i=0;i<n;i++){
            // If the current element is not zero
            if(nums[i]!=0){
                // Swap the non-zero element with the element at the 'idx' position
                swap(nums,i,idx);
                // Move the 'idx' pointer to the next position
                idx++;
            }
        }

    }
}
````