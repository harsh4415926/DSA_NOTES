# Bubble Sort
````java
/*
* ----------------------------------------------------------------------
* ALGORITHM: Bubble Sort
* ----------------------------------------------------------------------
* LOGIC (Adjacent Swapping):
* 1. The algorithm repeatedly steps through the list, compares adjacent
* elements, and swaps them if they are in the wrong order.
* 2. With each complete pass through the array, the largest unsorted
* element "bubbles up" to its correct position at the end of the array.
* 3. Because the end of the array becomes sorted first, the inner loop
* shrinks by 1 after each pass (j < n - i - 1).
* 4. OPTIMIZATION: If we make a complete pass through the array without
* making a single swap, it means the array is perfectly sorted, and we
* can break out early.
* ----------------------------------------------------------------------
*/

public static void bubbleSort(int[] arr) {
int n = arr.length;

    // The outer loop represents the number of passes. 
    // We need at most n - 1 passes to sort an array of size n.
    for (int i = 0; i < n - 1; i++) {
        
        // Flag to track if any swaps occurred during this specific pass
        boolean swapped = false;

        // The inner loop does the adjacent comparisons.
        // We subtract 'i' because the last 'i' elements are already sorted 
        // and safely in their final positions.
        for (int j = 0; j < n - i - 1; j++) {
            
            // If the left element is strictly greater than the right element...
            if (arr[j] > arr[j + 1]) {
                // ...swap them to push the larger element to the right
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
                
                // Mark that we had to make a change
                swapped = true;
            }
        }

        // Optimization: If the inner loop finished without a single swap, 
        // the array is fully sorted. We can safely stop early.
        if (!swapped) break;
    }
}

/*
* COMPLEXITY:
* Time:
* - Best Case: O(N). Because of the 'swapped' optimization, if the array is
* already sorted, it only takes one single pass to verify it.
* - Average/Worst Case: O(N^2). If the array is reverse sorted, the larger
* elements must be bubbled all the way to the end, step-by-step.
* Space: O(1). The sorting is done strictly in-place.
  */
````