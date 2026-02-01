# car pooling
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 1094 - Car Pooling
 * ----------------------------------------------------------------------
 * Determine if a car with capacity 'capacity' can pick up and drop off 
 * all passengers for all given trips.
 * * STEPS:
 * 1. Map events on a timeline: +passengers at start, -passengers at end.
 * 2. Use TreeMap to process locations in order.
 * 3. Sweep line: Update current load. If load > capacity, return false.
 * ----------------------------------------------------------------------
 */

class Solution {
    public boolean carPooling(int[][] trips, int capacity) {
        // Map: Location -> Net change in passengers
        // TreeMap ensures we process locations in ascending order (0, 1, 5, ...)
        TreeMap<Integer, Integer> map = new TreeMap<>();
        
        for (int i = 0; i < trips.length; i++) {
            int number = trips[i][0];
            int in = trips[i][1];
            int out = trips[i][2];
            
            // Passengers get ON at 'in'
            map.put(in, map.getOrDefault(in, 0) + number);
            
            // Passengers get OFF at 'out'
            // (Note: The problem implies passengers leave exactly at 'out', 
            // so capacity frees up at this specific coordinate).
            map.put(out, map.getOrDefault(out, 0) - number);
        }
        
        int currCost = 0;
        
        // Iterate through changes sorted by location
        for (int value : map.values()) {
            currCost += value;
            
            // If at any point current passengers exceed capacity, it's impossible
            if (currCost > capacity) {
                return false;
            }
        }
        
        return true;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N log N)
 * - N trips. TreeMap insertions take O(log N). Total N insertions.
 * - Iterating values takes O(N).
 * * Space Complexity: O(N)
 * - Map stores at most 2*N locations.
 * * Optimization Note: 
 * - Since location range is small (0 to 1000 usually), a simple array 
 * int[1001] would reduce Time to O(N) and Space to O(1) / O(Range).
 * ----------------------------------------------------------------------
 */
````