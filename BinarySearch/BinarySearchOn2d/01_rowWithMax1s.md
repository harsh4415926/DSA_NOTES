# row with max 1's(gfg)
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: Row with Maximum 1s (Sorted Rows)
 * ----------------------------------------------------------------------
 * LOGIC (Binary Search per Row):
 * 1. Since each row is sorted (0s followed by 1s), we don't need to count 
 * every element linearly.
 * 2. We can use Binary Search (Lower Bound) to find the index of the FIRST '1'.
 * 3. The number of 1s in that row is simply: (total columns) - (index of first 1).
 * 4. We iterate through all rows, keeping track of the row with the highest count.
 * ----------------------------------------------------------------------
 */

class Solution {
    
    // Helper: Finds the first index where arr[mid] >= target
    public int lowerBound(int[] arr, int target) {
        int low = 0;
        int high = arr.length - 1;
        int ans = arr.length; // Default to length if target (1) is not found
        
        while (low <= high) {
            int mid = low + (high - low) / 2;
            
            if (arr[mid] >= target) {
                ans = mid;        // Potential first occurrence, save it
                high = mid - 1;   // Look left for an even earlier occurrence
            } 
            else {
                low = mid + 1;    // Value is 0, look right
            }
        }
        
        return ans;
    }
    
    public int rowWithMax1s(int arr[][]) {
        int cnt = 0;      // Track the maximum number of 1s found so far
        int idx = -1;     // Track
````