# minimum days to make m bouquets
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 1482 - Minimum Number of Days to Make m Bouquets
 * ----------------------------------------------------------------------
 * LOGIC (Binary Search on Answer):
 * 1. Search Space: Days range from min(bloomDay) to max(bloomDay).
 * 2. `canMake` checks if `m` bouquets of `k` adjacent flowers can be 
 * harvested by a specific `day` using a Greedy approach.
 * 3. Binary Search:
 * - If possible by `mid` days, save it as a potential answer and try 
 * to find an even earlier day (search left half).
 * - If not possible, we must wait longer for more flowers to bloom 
 * (search right half).
 * ----------------------------------------------------------------------
 */

class Solution {

    // Helper: Checks if we can form 'm' bouquets of 'k' adjacent flowers by 'day'
    public boolean canMake(int[] bloomDay, int m, int k, int day) {
        int count = 0;      // Current sequence of bloomed flowers
        int bouquets = 0;   // Total successfully formed bouquets

        for (int i = 0; i < bloomDay.length; i++) {
            
            // Flower has bloomed by this day
            if (bloomDay[i] <= day) {
                count++;
                
                // If we've collected enough adjacent flowers for one bouquet
                if (count == k) {
                    bouquets++;
                    count = 0;  // Reset count for the next bouquet
                }
            } else {
                // Unbloomed flower breaks the adjacency chain
                count = 0;  
            }

            // Early exit optimization
            if (bouquets >= m) return true;
        }

        return false;
    }

    public int minDays(int[] bloomDay, int m, int k) {
        // Use long to prevent integer overflow (e.g., m = 10^5, k = 10^5)
        long totalFlowersNeeded = (long) m * k;
        
        // Edge case: Impossible to make required bouquets
        if (totalFlowersNeeded > bloomDay.length) return -1;

        int low = Integer.MAX_VALUE;
        int high = Integer.MIN_VALUE;

        // Establish the search boundaries based on the actual bloom days
        for (int day : bloomDay) {
            low = Math.min(low, day);
            high = Math.max(high, day);
        }

        int answer = -1;

        // Binary search for the optimal minimum day
        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (canMake(bloomDay, m, k, mid)) {
                answer = mid;       // This day works, save it
                high = mid - 1;     // Try an earlier day (search left)
            } else {
                low = mid + 1;      // Too early, need more days (search right)
            }
        }

        return answer;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N * log(max_day - min_day)) where N is the length of bloomDay.
 * Space: O(1) tracking scalar variables.
 */
````