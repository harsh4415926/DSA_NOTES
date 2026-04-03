# Selection Sort
````java
/*
* ----------------------------------------------------------------------
* ALGORITHM: Selection Sort
* ----------------------------------------------------------------------
* LOGIC (Find-and-Swap):
* 1. The algorithm divides the array into two parts: the sorted part at
* the left end and the unsorted part at the right end.
* 2. Initially, the sorted part is empty and the unsorted part is the
* entire array.
* 3. We iterate through the array. For each position 'i', we scan the
* ENTIRE remaining unsorted section to find the absolute smallest element.
* 4. Once we find the minimum element in the unsorted section, we swap
* it with the element at the current boundary 'i'.
* 5. This expands the sorted section by one element and shrinks the
* unsorted section.
* ----------------------------------------------------------------------
*/

public static void selectionSort(int[] arr) {
int n = arr.length;

    // The outer loop tracks the boundary between the sorted and unsorted sections.
    // We only need to go up to n - 1, because when only 1 element is left, 
    // it must already be the maximum and in the correct spot.
    for (int i = 0; i < n - 1; i++) {
        
        // Assume the first element of the unsorted section is the minimum
        int minIndex = i;

        // Scan the rest of the unsorted section to find the true minimum
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIndex]) {
                // We found a new smaller element, update our tracker
                minIndex = j;
            }
        }

        // Swap the lowest element we found with the first unsorted element at index 'i'
        // (Even if minIndex hasn't changed, a swap with itself is harmless)
        int temp = arr[minIndex];
        arr[minIndex] = arr[i];
        arr[i] = temp;
    }
}

/*
* COMPLEXITY:
* Time: O(N^2) for Best, Average, and Worst cases. Because it always blindly
* scans the entire unsorted section to find the minimum, it doesn't matter
* if the array is already sorted—it will still do the exact same number of comparisons.
* Space: O(1). The sorting is done in-place, requiring only a few variables
* ('minIndex', 'temp', 'i', 'j').
  */
````