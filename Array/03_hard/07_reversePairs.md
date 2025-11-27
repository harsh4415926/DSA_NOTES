# Problem: Reverse Pairs
You are given an integer array nums. A reverse pair is a pair of indices (i, j) such that 0 <= i < j < n (where n is the array length) and the condition nums[i] > 2 * nums[j] is met.

Your task is to find and return the total number of reverse pairs in the array. This solution uses a modified Merge Sort algorithm to efficiently count these pairs.

Example: Input: nums = [1, 3, 2, 3, 1] Output: 2 Explanation: The reverse pairs are (1, 4) (since 3 > 2 * 1) and (3, 4) (since 3 > 2 * 1).

---
````java
class Solution {
    int count = 0; // Global counter for reverse pairs

    /**
     * Merges two sorted subarrays and counts reverse pairs between them.
     * The first subarray is arr[low...mid]
     * The second subarray is arr[mid+1...high]
     * @param arr The main array.
     * @param low The starting index of the first subarray.
     * @param mid The ending index of the first subarray.
     * @param high The ending index of the second subarray.
     */
    public void merge(int[] arr, int low, int mid, int high) {
        // --- Start of Reverse Pair Counting ---
        int j = mid + 1;
        for (int i = low; i <= mid; i++) {
            // Find the first j such that arr[i] <= 2*arr[j]
            // We use 2L to prevent integer overflow during multiplication.
            while (j <= high && arr[i] > 2L * arr[j]) {
                j++;
            }
            // All elements from (mid+1) to (j-1) satisfy the condition.
            count += j - (mid + 1);
        }
        // --- End of Reverse Pair Counting ---

        // --- Standard Merge Sort Logic ---
        int i = low;
        j = mid + 1; // Reset j for the merge step
        List<Integer> temp = new ArrayList<>();
        
        while (i <= mid && j <= high) {
            if (arr[i] <= arr[j]) {
                temp.add(arr[i]);
                i++;
            } else {
                temp.add(arr[j]);
                j++;
            }
        }
        
        while (i <= mid) {
            temp.add(arr[i]);
            i++;
        }
        
        while (j <= high) {
            temp.add(arr[j]);
            j++;
        }
        
        // Copy sorted elements back to the original array
        for (i = low; i <= high; i++) {
            arr[i] = temp.get(i - low);
        }
    }

    /**
     * Recursive function to sort the array and count reverse pairs.
     * @param low The starting index of the segment.
     * @param high The ending index of the segment.
     * @param arr The array.
     */
    public void mergesort(int low, int high, int[] arr) {
        if (low >= high) return; // Base case
        
        int mid = (low + high) / 2;
        
        mergesort(low, mid, arr);     // Sort and count in left half
        mergesort(mid + 1, high, arr); // Sort and count in right half
        merge(arr, low, mid, high);   // Merge and count split-pairs
    }

  /**
     * Entry point for the reverse pairs counting algorithm.
     * @param nums The array to process.
     * @return The total number of reverse pairs.
     */
    public int reversePairs(int[] nums) {
        int n = nums.length;
        count = 0; // Reset global counter
        mergesort(0, n - 1, nums);
        return count;

        // Time Complexity: O(N log N)
        // - The 'mergesort' divides the array log N times.
        // - The 'merge' function has two parts:
        //   1. The reverse pair counting: O(N)
        //   2. The standard merge: O(N)
        // - Total complexity remains O(N log N).
        
        // Space Complexity: O(N)
        // - Required for the temporary 'ArrayList' during the merge step.
    }
}

````