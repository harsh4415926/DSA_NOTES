# Problem: Merge Sorted Array
You are given two integer arrays, nums1 and nums2, both sorted in non-decreasing order. You are also given their respective element counts, m and n.

nums1 has a total length of m + n, where the first m elements are the data to be merged, and the last n elements are placeholders (set to 0) that must be overwritten. nums2 has a length of n.

Your task is to merge nums2 into nums1 in-place, resulting in a single sorted array of length m + n. The solution should be efficient, ideally by filling nums1 from the end.

Example: Input: nums1 = [1, 2, 3, 0, 0, 0], m = 3, nums2 = [2, 5, 6], n = 3 Output: nums1 is modified to [1, 2, 2, 3, 5, 6]

---
````java
class Solution {
    /**
     * Merges two sorted arrays 'nums2' into 'nums1' in-place.
     * @param nums1 The first sorted array, with space at the end.
     * @param m The number of valid elements in nums1.
     * @param nums2 The second sorted array.
     * @param n The number of elements in nums2.
     */
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        // Pointer for the last valid element of nums1
        int i = m - 1; 
        // Pointer for the last element of nums2
        int j = n - 1;
        // Pointer for the last position in the nums1 array (where to place the largest element)
        int in = m + n - 1;

        // Loop backwards as long as there are elements in nums2 to merge
        while (j >= 0) {
            // Check if nums1 still has elements to compare
            if (i >= 0 && nums1[i] >= nums2[j]) {
                // If nums1's element is larger, place it at the end
                nums1[in] = nums1[i];
                i--;
            } else {
                // If nums2's element is larger or nums1 is exhausted, place nums2's element
                nums1[in] = nums2[j];
                j--;
            }
            // Move the placement index to the left
            in--;
        }
        // If j < 0, the loop ends. If i >= 0, the remaining elements in nums1
        // are already in their correct sorted position, so no further action is needed.

        // Time Complexity: O(m + n)
        // - We iterate through all 'n' elements of nums2 and 'm' elements of nums1
        //   in a single pass.

        // Space Complexity: O(1)
        // - The operation is performed in-place.
        // - We only use a few constant-space variables (pointers i, j, in).
    }
}

````