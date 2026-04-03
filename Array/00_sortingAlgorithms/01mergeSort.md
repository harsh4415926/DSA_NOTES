# Merge Sort 
````java
/*
 * ----------------------------------------------------------------------
 * ALGORITHM: Merge Sort
 * ----------------------------------------------------------------------
 * LOGIC (Divide and Conquer):
 * 1. Divide: Recursively divide the array into two halves until you 
 * reach subarrays of size 1 (which are inherently sorted).
 * 2. Conquer (Merge): Merge the smaller sorted subarrays back together.
 * 3. The `merge` step uses two temporary arrays (`leftArr` and `rightArr`) 
 * to hold the separated data.
 * 4. We use a Two-Pointer approach to iterate through both temporary 
 * arrays, comparing elements and overwriting the original array with 
 * the elements in perfectly sorted order.
 * ----------------------------------------------------------------------
 */

public static void mergeSort(int[] arr, int left, int right) {
    // Base case: If the subarray has 1 or 0 elements, it is already sorted
    if (left >= right) return;

    // Find the middle point to divide the array into two halves.
    // (left + right) / 2 can cause integer overflow for large arrays, 
    // so left + (right - left) / 2 is the safer standard.
    int mid = left + (right - left) / 2;

    // Recursively sort the first half and the second half
    mergeSort(arr, left, mid);
    mergeSort(arr, mid + 1, right);

    // Merge the two sorted halves back together
    merge(arr, left, mid, right);
}

private static void merge(int[] arr, int left, int mid, int right) {
    // Calculate the sizes of the two subarrays to be merged
    int n1 = mid - left + 1;
    int n2 = right - mid;

    // Create temporary arrays
    int[] leftArr = new int[n1];
    int[] rightArr = new int[n2];

    // Copy data to the temporary arrays
    for (int i = 0; i < n1; i++)
        leftArr[i] = arr[left + i];

    for (int j = 0; j < n2; j++)
        rightArr[j] = arr[mid + 1 + j];

    // Initial indices for the left (i), right (j), and merged (k) arrays
    int i = 0, j = 0, k = left;

    // Traverse both arrays and pick the smaller element to place into arr[]
    while (i < n1 && j < n2) {
        if (leftArr[i] <= rightArr[j]) {
            arr[k++] = leftArr[i++];
        } else {
            arr[k++] = rightArr[j++];
        }
    }

    // Copy any remaining elements of leftArr[] (if any are left)
    while (i < n1) {
        arr[k++] = leftArr[i++];
    }

    // Copy any remaining elements of rightArr[] (if any are left)
    while (j < n2) {
        arr[k++] = rightArr[j++];
    }
}

/*
 * COMPLEXITY:
 * Time: O(N log N). The array is continuously divided in half (log N levels), 
 * and merging at each level takes O(N) time. This remains true for worst, 
 * best, and average cases.
 * Space: O(N). We allocate temporary arrays `leftArr` and `rightArr` during 
 * the merge process, proportional to the size of the array.
 */
````