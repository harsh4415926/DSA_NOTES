# nth root of m (gfg)
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: Find N-th Root of a Number
 * ----------------------------------------------------------------------
 * LOGIC (Binary Search): Search Space: [0, m]
 * 1. Compute mid = low + (high-low)/2.
 * 2. Calculate value = mid^n.
 * 3. Adjust boundaries:
 * - If value == m -> Found the root.
 * - If value < m -> Root is larger (low = mid + 1).
 * - If value > m -> Root is smaller (high = mid - 1).
 * 4. Critical Addition: Prevent overflow when calculating mid^n by 
 * breaking early if result > m.
 * ----------------------------------------------------------------------
 */

class Solution {
    
    // Function to compute (mid^n) safely without Long overflow
    public static long power(long mid, int n, long m) {
        long result = 1;
        
        for (int i = 0; i < n; i++) {
            result *= mid;
            
            // Overflow prevention: If at any point result exceeds target 'm',
            // return it immediately so binary search can adjust bounds.
            if (result > m) {
                return result; 
            }
        }
        
        return result;
    }
    
    public static int nthRoot(int n, int m) {
        
        int low = 0;
        int high = m;
        
        // Binary Search
        while (low <= high) {
            
            // Prevent integer overflow when adding low + high
            int mid = low + (high - low) / 2;
            
            long value = power(mid, n, m);
            
            if (value == m) {
                return mid; // Exact match found
            }
            else if (value < m) {
                low = mid + 1; // Need a larger number
            }
            else {
                high = mid - 1; // Need a smaller number
            }
        }
        
        // N-th root is not an integer
        return -1;
    }
}

/*
 * COMPLEXITY:
 * Time: O(n * log(m)) (Binary search log(m) times, each step takes up to O(n) for power).
 * Space: O(1) (Only tracking a few variables).
 */
````
