# painters partition problem 
## same as allocate minimum pages

````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: Painter's Partition Problem / Split Array Largest Sum
 * ----------------------------------------------------------------------
 * LOGIC (Binary Search on Answer):
 * 1. This is functionally identical to the "Book Allocation" problem.
 * 2. Goal: Partition the array into 'k' contiguous subarrays such that 
 * the maximum sum of any subarray is MINIMIZED.
 * 3. Search Space: 
 * - Lowest possible time: max(arr) (A painter must take at least the 
 * largest single board).
 * - Highest possible time: sum(arr) (One painter paints everything).
 * 4. `painterRequired` strategy (Greedy): Keep assigning boards to the 
 * current painter until adding the next board exceeds 'maxTime'.
 * 5. Binary Search:
 * - If we can finish the job with <= 'k' painters, this 'mid' time is 
 * valid. Record it and try for a strictly smaller max time (search left).
 * - If we need > 'k' painters, the time limit is too strict. We need 
 * to allow more time per painter (search right).
 * ----------------------------------------------------------------------
 */

class Solution {
    
    // Helper: Returns the number of painters needed if the max time allowed is 'maxTime'
    public int painterRequired(int maxTime, int[] arr) {
        int painter = 1;      // Start with the first painter
        int currTime = 0;     // Time spent by the current painter

        for (int i = 0; i < arr.length; i++) {
            
            // If adding the next board keeps them within the time limit
            if (currTime + arr[i] <= maxTime) {
                currTime += arr[i];  
            } 
            else {
                // Limit exceeded: Assign this board to a NEW painter
                painter++;
                currTime = arr[i]; // New painter starts with this board
            }
        }

        return painter;
    }

    public int minTime(int[] arr, int k) {

        int low = 0;
        int sum = 0;

        // Establish boundaries for the binary search
        for (int i = 0; i < arr.length; i++) {
            low = Math.max(low, arr[i]); // Min possible answer (largest single board)
            sum += arr[i];               // Max possible answer (sum of all boards)
        }

        int high = sum;
        int ans = sum;

        // Binary search for the MINIMUM possible maximum time
        while (low <= high) {
            int mid = low + (high - low) / 2;

            // If the required painters for this 'mid' time is within our limit 'k'
            if (painterRequired(mid, arr) <= k) {
                ans = mid;        // 'mid' works, save it
                high = mid - 1;   // Try to push the maximum time even lower (search left)
            } 
            else {
                low = mid + 1;    // 'mid' is too small, need a larger time limit (search right)
            }
        }

        return ans;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N * log(sum - max)) where N is the length of the array.
 * Space: O(1) tracking scalar variables.
 */
````