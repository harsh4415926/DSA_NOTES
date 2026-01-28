# maximum population year
## using difference array
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 1854 - Maximum Population Year
 * ----------------------------------------------------------------------
 * Find the earliest year with the maximum population given birth/death logs.
 * * STEPS:
 * 1. Use a Difference Array: +1 at birth year, -1 at death year.
 * 2. Calculate Prefix Sums to get actual population per year & track maximum.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int maximumPopulation(int[][] logs) {
        // Array to store population changes. Range 1950-2050.
        // Size 2051 covers indices up to 2050.
        int[] diff = new int[2051];
        
        // 1. Build Difference Array (Line Sweep)
        for (int i = 0; i < logs.length; i++) {
            int birth = logs[i][0];
            int death = logs[i][1];
            
            diff[birth] += 1; // Population increases at birth
            diff[death] -= 1; // Population decreases at death (death year is exclusive in problem usually, or inclusive drop)
                              // Problem states: "dead on year X means not alive during X", so drop at X.
        }
        
        int currPopulation = 0;
        int maxPopulation = Integer.MIN_VALUE;
        int maxPopulationYear = 0;
        
        // 2. Prefix Sum to find Max Population
        // Iterate only through the valid year range given in constraints
        for (int i = 1950; i < 2051; i++) {
            currPopulation += diff[i]; // Running sum gives current population
            
            // strictly greater updates answer (ensures earliest year is kept for ties)
            if (currPopulation > maxPopulation) {
                maxPopulation = currPopulation;
                maxPopulationYear = i;
            }
        }
        return maxPopulationYear;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N + Y)
 * - N = number of logs (to build array), Y = range of years (to traverse array, ~100).
 * * Space Complexity: O(Y)
 * - Fixed size array for the year range (2051).
 * ----------------------------------------------------------------------
 */
````
## using line sweep
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 1854 - Maximum Population Year (Sorting/Sweep Line)
 * ----------------------------------------------------------------------
 * Find earliest year with max population using Event Sorting.
 * * STEPS:
 * 1. Create events: (BirthYear, +1) and (DeathYear, -1).
 * 2. Sort events by Year. Tie-breaker: Process Death (-1) before Birth (+1).
 * 3. Iterate events -> Update running population sum -> Track maximum.
 * ----------------------------------------------------------------------
 */

class Pair {
    int first;  // Year
    int second; // Type (+1 or -1)
    
    Pair(int first, int second) {
        this.first = first;
        this.second = second;
    }
}

class Solution {
    public int maximumPopulation(int[][] logs) {
        List<Pair> events = new ArrayList<>();
        
        // 1. Generate Events
        for (int i = 0; i < logs.length; i++) {
            int birth = logs[i][0];
            int death = logs[i][1];
            
            // Add +1 for birth, -1 for death (since death year is exclusive)
            events.add(new Pair(birth, 1));
            events.add(new Pair(death, -1));
        }
        
        // 2. Sort Events
        Collections.sort(events, (a, b) -> {
            // Primary Sort: By Year (ascending)
            if (a.first != b.first) {
                return a.first - b.first;
            } 
            // Secondary Sort: By Type
            // We want Death (-1) to come before Birth (1) if years are equal.
            // Why? To avoid false peaks. If someone dies in 2000 (not alive) 
            // and someone is born in 2000 (alive), we drop the count before adding.
            else {
                return a.second - b.second; // -1 < 1, so death comes first
            }
        });

        int currSum = 0;
        int maxSum = 0;
        int maxSumYear = 0;
        
        // 3. Line Sweep
        for (int i = 0; i < events.size(); i++) {
            currSum += events.get(i).second;
            
            // Update max if we found a strictly larger population
            if (currSum > maxSum) {
                maxSum = currSum;
                maxSumYear = events.get(i).first;
            }
        }
        return maxSumYear;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N log N)
 * - Sorting the events list dominates the complexity (where N is number of logs).
 * * Space Complexity: O(N)
 * - We create a list of size 2*N to store birth and death events.
 * ----------------------------------------------------------------------
 */
````
