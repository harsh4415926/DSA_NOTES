# aggressive cows
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: Aggressive Cows (Maximize Minimum Distance)
 * ----------------------------------------------------------------------
 * LOGIC (Binary Search on Answer):
 * 1. Sort the stalls so we can greedily place cows from left to right.
 * 2. Search Space for the minimum distance: 
 * - Lowest possible distance: 1
 * - Highest possible distance: stalls[n-1] - stalls[0]
 * 3. `canPlace` strategy (Greedy): Always place the first cow in the 
 * first stall. For subsequent cows, place them in the next available 
 * stall that is at least `minDist` away from the previously placed cow.
 * 4. Binary Search:
 * - If `mid` is a valid minimum distance, record it and try to find an 
 * even LARGER minimum distance (search right).
 * - If we cannot place all cows with `mid` distance, `mid` is too large 
 * (search left).
 * ----------------------------------------------------------------------
 */

import java.util.*;

class Solution {

    // Helper: Checks if we can place 'totalCows' with at least 'minDist' gap
    public boolean canPlace(int minDist, int totalCows, int[] stalls) {
        int cows = 1;             // Place the first cow
        int last = stalls[0];     // Track the position of the last placed cow

        for (int i = 1; i < stalls.length; i++) {
            // If the current stall is far enough from the last placed cow
            if (stalls[i] - last >= minDist) {
                cows++;           // Place another cow
                last = stalls[i]; // Update the position of the last placed cow
            }
            
            // Early exit: Successfully placed all cows
            if (cows >= totalCows) return true;
        }

        return false; // Could not place all cows with the required distance
    }

    public int aggressiveCows(int[] stalls, int k) {
        // MUST sort the array for the greedy placement strategy to work
        Arrays.sort(stalls);

        int n = stalls.length;
        int low = 1; 
        int high = stalls[n - 1] - stalls[0]; 
        int ans = 0;

        // Binary search for the MAXIMUM possible minimum distance
        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (canPlace(mid, k, stalls)) {
                ans = mid;        // 'mid' is a valid distance, save it
                low = mid + 1;    // Try to maximize the distance (search right)
            } else {
                high = mid - 1;   // 'mid' is too large, need a smaller gap (search left)
            }
        }

        return ans;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N log N) (Sorting) + O(N * log(max_dist)) (Binary Search).
 * Space: O(log N) or O(1) depending on the sorting algorithm overhead.
 */
````