# fractional knapsack(gfg)
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: Fractional Knapsack
 * ----------------------------------------------------------------------
 * LOGIC (Greedy Strategy):
 * 1. Calculate Value/Weight ratio for each item.
 * 2. Sort items in DESCENDING order of this ratio. (We want the most 
 * "bang for our buck" first).
 * 3. Iterate through sorted items:
 * - If the item fits entirely, take it and reduce capacity.
 * - If it doesn't fit entirely, take the fractional amount that fills 
 * the remaining capacity and stop (knapsack is full).
 * ----------------------------------------------------------------------
 */

import java.util.*;

class Solution {
    public double fractionalKnapsack(int[] val, int[] wt, int capacity) {
        
        int n = val.length;
        
        // Use an Integer array to store original indices. 
        // This avoids creating a custom class just for sorting.
        Integer[] index = new Integer[n];
        for (int i = 0; i < n; i++) {
            index[i] = i;
        }
        
        // Sort the indices based on value/weight ratio in descending order.
        // Using Double.compare prevents precision loss and overflow issues.
        Arrays.sort(index, (a, b) -> 
            Double.compare((double) val[b] / wt[b],
                           (double) val[a] / wt[a]));
        
        double totalValue = 0.0;
        
        // Greedily pick items
        for (int i = 0; i < n; i++) {
            int idx = index[i]; // Get the index of the next best item
            
            if (capacity >= wt[idx]) {
                // Case 1: Enough capacity to take the full item
                totalValue += val[idx];
                capacity -= wt[idx];
            } else {
                // Case 2: Only partial capacity left. Take fractional part.
                // Formula: (Unit Value) * (Remaining Capacity)
                totalValue += ((double) val[idx] / wt[idx]) * capacity;
                break;  // Knapsack is completely full now
            }
        }
        
        return totalValue;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N log N) (Sorting the index array dominates the time).
 * Space: O(N) (For the Integer index array).
 */
````