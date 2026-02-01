## fixed size Window (eg. Find the maximum sum of any contiguous subarray of size $k$)
````java
public int fixedSlidingWindow(int[] arr, int k) {
    // Edge case: array smaller than window
    if (arr.length < k) return -1;

    int currentSum = 0;

    // 1. Initialize the first window
    for (int i = 0; i < k; i++) {
        currentSum += arr[i];
    }
    
    int maxSum = currentSum;

    // 2. Slide the window
    // Start loop from the k-th element
    for (int i = k; i < arr.length; i++) {
        // Add the incoming element: arr[i]
        // Subtract the outgoing element: arr[i - k]
        currentSum += arr[i] - arr[i - k];
        
        maxSum = Math.max(maxSum, currentSum);
    }

    return maxSum;
}
````

## variable size window
````java
public int variableSlidingWindow(int[] arr) {
    int left = 0;
    int currentResult = 0; 
    int bestResult = 0; // Use Integer.MIN_VALUE or MAX_VALUE depending on problem

    for (int right = 0; right < arr.length; right++) {
        // 1. Add the right element to the window (Expand)
        // logic: currentResult += arr[right]; 
        
        // 2. Shrink the window if the condition is broken
        while (/* CONDITION_BROKEN */) {
            // Remove the left element from logic
            // logic: currentResult -= arr[left];
            left++;
        }

        // 3. Update the best result
        // Window size is always: (right - left + 1)
        bestResult = Math.max(bestResult, right - left + 1);
    }

    return bestResult;
}
````


