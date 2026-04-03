# Insertion Sort
````java
/*
 * ----------------------------------------------------------------------
 * ALGORITHM: Insertion Sort
 * ----------------------------------------------------------------------
 * LOGIC (Incremental Sorting):
 * 1. The algorithm builds a sorted section of the array one element at 
 * a time, growing from left to right. It is exactly like sorting playing 
 * cards in your hands.
 * 2. We assume the very first element (index 0) is already "sorted".
 * 3. We iterate from the second element (index 1) to the end. The current 
 * element is our 'key'.
 * 4. We compare the 'key' to the elements in the sorted section (moving 
 * backwards from i-1 to 0).
 * 5. If a sorted element is greater than our 'key', we shift that sorted 
 * element one position to the right to make room.
 * 6. Once we find an element smaller than the 'key' (or reach the start 
 * of the array), we drop the 'key' into that newly created empty space.
 * ----------------------------------------------------------------------
 */

public static void insertionSort(int[] arr) {
    int n = arr.length;

    // Start from index 1 because a single element (index 0) is trivially sorted
    for (int i = 1; i < n; i++) {
        
        // The element we are currently trying to place in the correct spot
        int key = arr[i];
        
        // 'j' points to the last element of the currently sorted section
        int j = i - 1;

        // Shift elements of the sorted section that are greater than the key
        // to one position ahead of their current position
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j]; // Shift right
            j--;                 // Move pointer backwards
        }

        // Place the key into the correct position. 
        // We use j + 1 because the while loop decrements j one final time 
        // before terminating.
        arr[j + 1] = key;
    }
}

/*
 * COMPLEXITY:
 * Time: 
 * - Best Case: O(N). If the array is already sorted, the while loop never runs.
 * - Average/Worst Case: O(N^2). If the array is reverse sorted, we must shift 
 * every single element for every key.
 * Space: O(1). The sorting is done entirely in-place, requiring only a few 
 * scalar variables ('key', 'i', 'j').
 */
````