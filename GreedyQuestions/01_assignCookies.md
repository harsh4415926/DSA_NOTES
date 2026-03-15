# assign cookies
````java
/*
* ----------------------------------------------------------------------
* PROBLEM: LeetCode 455 - Assign Cookies
* ----------------------------------------------------------------------
* LOGIC (Greedy Strategy):
* 1. Sort both arrays: Start with the least greedy child and smallest cookie.
* 2. Two Pointers: 'i' for children (g), 'j' for cookies (s).
* 3. Match: If cookie j >= child i, assign it (i++, j++).
* 4. Skip: If cookie is too small, move to next larger cookie (j++).
* ----------------------------------------------------------------------
*/

import java.util.*;

class Solution {
public int findContentChildren(int[] g, int[] s) {
Arrays.sort(g); // Sort children by greed factor
Arrays.sort(s); // Sort cookies by size

        int i = 0; // Pointer for child
        int j = 0; // Pointer for cookie
        
        // Iterate until we run out of children or cookies
        while (i < g.length && j < s.length) {
            // Greedy Check: If current cookie satisfies current child
            if (s[j] >= g[i]) {
                i++; // Child satisfied, move to next child
            }
            j++; // Always move to next cookie (used or too small)
        }
        
        return i; // 'i' represents count of satisfied children
    }
}

/*
* COMPLEXITY:
* Time: O(N log N + M log M) (Sorting dominates).
* Space: O(log N) (Sorting stack space).
  */
````