# merge Sort
````java
import java.util.ArrayList;
import java.util.List;

class Solution {
    
    /**
     * Merges two sorted subarrays into one sorted subarray.
     * The first subarray is arr[low...mid]
     * The second subarray is arr[mid+1...high]
     * @param arr The main array.
     * @param low The starting index of the first subarray.
     * @param mid The ending index of the first subarray.
     * @param high The ending index of the second subarray.
     */
    public void merge(int[] arr, int low, int mid, int high) {
        int i = low;      // Pointer for the first half
        int j = mid + 1;  // Pointer for the second half
        List<Integer> temp = new ArrayList<>(); // Temporary list to store merged elements

        // Compare elements from both halves and add the smaller one to temp
        while (i <= mid && j <= high) {
            if (arr[i] <= arr[j]) {
                temp.add(arr[i]);
                i++;
            } else {
                temp.add(arr[j]);
                j++;
            }
        }

        // If elements are left in the first half, add them
        while (i <= mid) {
            temp.add(arr[i]);
            i++;
        }

        // If elements are left in the second half, add them
        while (j <= high) {
            temp.add(arr[j]);
            j++;
        }

        // Copy the sorted elements from temp back into the original array
        for (i = low; i <= high; i++) {
            arr[i] = temp.get(i - low);
        }
    }

    /**
     * Recursive function to sort the array using merge sort.
     * @param low The starting index of the segment to sort.
     * @param high The ending index of the segment to sort.
     * @param arr The array to be sorted.
     */
    public void mergesort(int low, int high, int[] arr) {
        // Base case: if the segment has 0 or 1 elements, it's already sorted
        if (low >= high) return;
        
        int mid = (low + high) / 2; // Find the middle point
        
        // Recursively sort the first half
        mergesort(low, mid, arr);
        
        // Recursively sort the second half
        mergesort(mid + 1, high, arr);
        
        // Merge the two sorted halves
        merge(arr, low, mid, high);
    }

    /**
     * Entry point for the merge sort algorithm.
     * @param arr The array to be sorted.
     * @param l The starting index (usually 0).
     * @param r The ending index (usually arr.length - 1).
     */
    void mergeSort(int arr[], int l, int r) {
        // code here
        int n = arr.length;
        // Start the recursive sort
        mergesort(l, r, arr);

        // Time Complexity: O(N log N)
        // - The 'mergesort' function divides the array log N times.
        // - At each level of division, the 'merge' function processes N elements.
        
        // Space Complexity: O(N)
        // - The 'merge' function creates a temporary 'ArrayList' of size up to N
        //   to store the merged elements.
    }
}
````