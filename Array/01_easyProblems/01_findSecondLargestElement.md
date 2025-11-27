# Problem: Find Second Largest Element
Given an array of integers arr, your task is to find the second largest distinct element in the array. If a second largest element does not exist (e.g., all elements are the same), you should return -1.

Example:

Input: arr = [12, 35, 1, 10, 34, 1]

Output: 34

Explanation: The largest element is 35, and the second largest is 34.

---
````java
class Solution {
    public int getSecondLargest(int[] arr) {
        // Initialize largest and second largest to the smallest possible integer value.
        int largest = Integer.MIN_VALUE;
        int secondLargest = Integer.MIN_VALUE;

        // Iterate through the entire array.
        for (int i = 0; i < arr.length; i++) {
            // If the current element is greater than the largest...
            if (arr[i] > largest) {
                // ...the old largest becomes the new second largest.
                secondLargest = largest;
                // ...and the current element becomes the new largest.
                largest = arr[i];
            }
            // If the element is not the largest, but is greater than the second largest.
            else if (arr[i] > secondLargest && arr[i] != largest) {
                // Update the second largest.
                secondLargest = arr[i];
            }
        }
        
        // If secondLargest was never updated, it means no second largest element exists.
        if (secondLargest == Integer.MIN_VALUE) return -1;
        
        return secondLargest;
    }

    // Time Complexity: O(N)
    // - We iterate through the array only once.

    // Space Complexity: O(1)
    // - We only use a few constant variables.
}
````
