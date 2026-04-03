# Quick Sort
````java
/*
 * ----------------------------------------------------------------------
 * ALGORITHM: Quick Sort (Lomuto Partition Scheme)
 * ----------------------------------------------------------------------
 * LOGIC (Divide and Conquer + In-Place Sorting):
 * 1. Divide: Pick a "pivot" element (in this case, the last element).
 * 2. Partition: Rearrange the array so that all elements smaller than 
 * or equal to the pivot are on its left, and all larger elements are 
 * on its right. The pivot is now in its final, perfectly sorted position.
 * 3. Conquer: Recursively apply the same process to the left sub-array 
 * and the right sub-array.
 * 4. The partitioning uses two pointers:
 * - 'j' scans through the array to find elements smaller than the pivot.
 * - 'i' keeps track of the boundary of the "smaller than pivot" section.
 * Whenever 'j' finds a smaller element, we expand the boundary (i++) 
 * and swap the element at 'j' into that smaller section.
 * ----------------------------------------------------------------------
 */

public static void quickSort(int[] arr, int low, int high) {
    // Base case: If the sub-array has 1 or 0 elements, it's already sorted
    if (low >= high) return;

    // Partition the array and get the final sorted index of the pivot
    int pivotIndex = partition(arr, low, high);

    // Recursively sort the elements BEFORE the pivot
    quickSort(arr, low, pivotIndex - 1);
    
    // Recursively sort the elements AFTER the pivot
    quickSort(arr, pivotIndex + 1, high);
}

private static int partition(int[] arr, int low, int high) {
    // Standard Lomuto partition: Choose the last element as the pivot
    int pivot = arr[high]; 
    
    // 'i' marks the end index of the window containing elements smaller than the pivot
    int i = low - 1;

    // 'j' iterates through the current sub-array
    for (int j = low; j < high; j++) {
        
        // If the current element is smaller than or equal to the pivot...
        if (arr[j] <= pivot) {
            i++; // Expand the "smaller elements" window
            
            // Swap arr[i] and arr[j] to push the smaller element into the window
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
        }
    }

    // Finally, place the pivot exactly after the "smaller elements" window.
    // This puts the pivot in its absolute correct sorted position.
    int temp = arr[i + 1];
    arr[i + 1] = arr[high];
    arr[high] = temp;

    // Return the final index of the pivot so we can split the array around it
    return i + 1;
}

/*
 * COMPLEXITY:
 * Time: 
 * - Average/Best Case: O(N log N). The pivot roughly splits the array in half each time.
 * - Worst Case: O(N^2). Happens if the array is already sorted (or reverse sorted) 
 * and we consistently pick the worst pivot (the last element).
 * Space: O(log N) average, O(N) worst case. This is entirely due to the recursion 
 * call stack. Quick sort is an "in-place" sort, so it doesn't need external arrays like Merge Sort.
 */
````