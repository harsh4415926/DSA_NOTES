# minimum max distance to gas stations (gfg)

----------------------------

## greedy approach +minheap
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: Minimize Max Distance to Gas Station
 * ----------------------------------------------------------------------
 * LOGIC (Greedy + Max-Heap):
 * 1. Use a Priority Queue (Max-Heap) to always target the largest gap 
 * between any two stations.
 * 2. Tuple stores: {section_index, current_gap_distance, stations_added}.
 * 3. Initialize the heap with the original gaps between adjacent stations.
 * 4. Greedily place 'K' stations:
 * - Extract the largest gap.
 * - Add 1 more station to this gap.
 * - Recalculate the gap distance: original_gap_length / (1 + total_stations_added).
 * - Push the updated gap back into the heap.
 * 5. After placing all K stations, the top of the heap is the minimized max distance.
 * ----------------------------------------------------------------------
 */

import java.util.*;

class Tuple {
    int first;     // Original index 'i' (represents gap between i and i+1)
    double second; // Current maximum distance within this gap
    int third;     // Number of new stations already placed in this gap
    
    Tuple(int first, double second, int third) {
        this.first = first;
        this.second = second;
        this.third = third;
    }
}

class Solution {
    public double minMaxDist(int[] stations, int K) {
        int n = stations.length;
        if (n <= 1) return 0.0;
        
        // Max-Heap sorted by the current gap distance (descending)
        PriorityQueue<Tuple> pq = new PriorityQueue<>((a, b) -> Double.compare(b.second, a.second));
        
        // Step 1: Push all initial gaps into the Priority Queue
        for (int i = 0; i < n - 1; i++) {
            double dist = (double) stations[i + 1] - stations[i];
            pq.add(new Tuple(i, dist, 0));
        }
        
        // Step 2: Greedily place K stations into the largest available gaps
        for (int i = 1; i <= K; i++) { 
             
            // Get the segment with the largest current gap
            Tuple t = pq.poll();
            int idx = t.first;
            // double dist = t.second; // (Not strictly needed for recalculation)
            int station = t.third;
             
            // Add a new station to this segment
            int newStations = station + 1;
            
            // Recalculate the distance: 
            // Total original distance of the segment / total partitions
            // (e.g., 1 station splits the gap into 2 partitions)
            double newDist = ((double) stations[idx + 1] - stations[idx]) / (1 + newStations);
            
            // Put it back in the heap
            pq.add(new Tuple(idx, newDist, newStations));
        }
        
        // The maximum distance remaining is at the top of the heap
        return pq.peek().second;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N log N) to build heap + O(K log N) to place K stations -> Total: O((N + K) log N).
 * Space: O(N) for the Priority Queue.
 */
````

---------------------
## binary search  
````java

/*
 * ----------------------------------------------------------------------
 * PROBLEM: Minimize Max Distance to Gas Station (Optimal)
 * ----------------------------------------------------------------------
 * LOGIC (Binary Search on Answer with Floating Point):
 * 1. Goal: Find the minimum possible maximum gap between stations after 
 * adding 'K' new stations.
 * 2. Search Space: 
 * - Lowest possible max distance: 0
 * - Highest possible max distance: The largest existing gap.
 * 3. `requiredStations` strategy: For a given target distance `dist`, 
 * how many stations do we need to insert in a `gap` to ensure no sub-gap 
 * exceeds `dist`? 
 * - Formula: ceil(gap / dist) - 1. 
 * (e.g., gap = 10, dist = 3. 10/3 = 3.33 -> ceil = 4 partitions -> 3 stations).
 * 4. Binary Search (Continuous):
 * - Because the answer is a double, we search until the difference 
 * between 'high' and 'low' is infinitesimally small (e.g., 1e-6).
 * - If we need > K stations, 'mid' is too tight (too small). Search right (`low = mid`).
 * - If we need <= K stations, 'mid' is valid. Record it and try tighter (search left, `high = mid`).
 * ----------------------------------------------------------------------
 */

class Solution {

    // Helper: Calculates total stations needed to ensure max gap <= 'dist'
    public int requiredStations(int[] stations, double dist) {
        int count = 0;

        for (int i = 0; i < stations.length - 1; i++) {
            double gap = stations[i + 1] - stations[i];
            
            // To divide 'gap' into segments of max size 'dist', we need
            // ceil(gap/dist) segments, which requires ceil(gap/dist) - 1 stations.
            count += (int) Math.ceil(gap / dist) - 1;
        }

        return count;
    }

    public double minMaxDist(int[] stations, int K) {
        int n = stations.length;

        double low = 0;
        double high = 0;

        // Establish the upper bound (the largest original gap)
        for (int i = 0; i < n - 1; i++) {
            high = Math.max(high, stations[i + 1] - stations[i]);
        }
        
        double ans = high;
        
        // Floating-point binary search precision limit (up to 6 decimal places)
        while (high - low > 1e-6) {

            double mid = low + (high - low) / 2;

            // If we need more stations than we have, this distance is too strict
            if (requiredStations(stations, mid) > K) {
                low = mid; // Need a larger allowed gap
            } 
            else {
                // We have enough stations to achieve this max distance (or better)
                ans = mid;
                high = mid; // Try to squeeze the max distance even smaller
            }
        }

        return ans;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N * log2(max_gap / 1e-6)). This is extremely fast and completely independent of K.
 * Space: O(1) tracking scalar variables.
 */

````
