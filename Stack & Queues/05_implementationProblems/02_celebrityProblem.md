# celebrity Problem
## using O(n<sup>2</sup>) time Complexity
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: The Celebrity Problem (Brute Force)
 * ----------------------------------------------------------------------
 * Find a person (celebrity) who is known by everyone else but knows no one.
 * * STEPS:
 * 1. Calculate indegree (knownTo) and outdegree (knows) for each person.
 * 2. Check if there exists a person i such that outdegree[i] == 0 & indegree[i] == n-1.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int celebrity(int mat[][]) {
        int n = mat.length;
        int[] knows = new int[n];    // Outdegree: How many people 'i' knows
        int[] knownTo = new int[n];  // Indegree: How many people know 'i'

        // 1. Build Degrees
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (i == j) continue; // Self-loops don't count
                if (mat[i][j] == 1) {
                    knows[i]++;      // i knows j
                    knownTo[j]++;    // j is known by i
                }
            }
        }
        
        // 2. Find Celebrity
        for (int i = 0; i < n; i++) {
            // A celebrity must know 0 people AND be known by all other (n-1) people
            if (knows[i] == 0 && knownTo[i] == n - 1) {
                return i;
            }
        }
        return -1;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N^2)
 * - We iterate through the entire NxN matrix.
 * - (Note: This can be optimized to O(N) using Two Pointers or a Stack).
 * * Space Complexity: O(N)
 * - Two arrays of size N.
 * ----------------------------------------------------------------------
 */
````

## optimised O(n<sup>2</sup>) time complexity (two pointers)
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: The Celebrity Problem (Two Pointers / Optimal)
 * ----------------------------------------------------------------------
 * Identify the celebrity (known by all, knows no one) in O(N) time.
 * * STEPS:
 * 1. Use two pointers (top, bottom) to eliminate non-celebrities based on "knows" relationship.
 * 2. If 'A' knows 'B', 'A' is not celebrity. If neither knows each other, both aren't.
 * 3. Verify the final candidate by checking their row (all 0s) and column (all 1s).
 * ----------------------------------------------------------------------
 */

class Solution {
    public int celebrity(int mat[][]) {
        int n = mat.length;
        
        int top = 0;
        int bottom = n - 1;
        
        // STEP 1: Elimination Phase
        while (top < bottom) {
            if (mat[top][bottom] == 1) {
                top++; // Top knows someone -> Top is NOT celebrity
            } 
            else if (mat[bottom][top] == 1) {
                bottom--; // Bottom knows someone -> Bottom is NOT celebrity
            } 
            else {
                // Neither knows the other -> Neither is a celebrity
                // (Because a celebrity must be known by everyone)
                top++;
                bottom--;
            }
        }
        
        // If pointers crossed without meeting, no candidate remains
        if (top > bottom) return -1;
        
        // STEP 2: Verification Phase
        // Check if the remaining candidate ('top') actually satisfies the conditions
        for (int i = 0; i < n; i++) {
            if (top == i) continue;
            
            // Invalid if:
            // 1. Candidate knows someone (mat[top][i] == 1)
            // 2. Someone doesn't know candidate (mat[i][top] == 0)
            if (mat[top][i] == 1 || mat[i][top] == 0) {
                return -1;
            }
        }
        return top;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N)
 * - The elimination phase is O(N) as pointers move towards each other.
 * - The verification phase is O(N).
 * * Space Complexity: O(1)
 * - No extra data structures used.
 * ----------------------------------------------------------------------
 */
````
