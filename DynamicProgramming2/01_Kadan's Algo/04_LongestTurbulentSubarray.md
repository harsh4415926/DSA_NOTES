````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 978 - Longest Turbulent Subarray
 * ----------------------------------------------------------------------
 * LOGIC (State Machine / 1D Dynamic Programming):
 * 1. A turbulent array alternates directions: UP, DOWN, UP, DOWN...
 * 2. We track two states for the subarray ending at the current index 'i':
 * - 'inc': The length of the turbulent subarray ending with an UP move (arr[i-1] < arr[i]).
 * - 'dec': The length of the turbulent subarray ending with a DOWN move (arr[i-1] > arr[i]).
 * 3. State Transitions:
 * - If we go UP, it perfectly extends a sequence that previously went DOWN. So, inc = dec + 1.
 * - If we go DOWN, it perfectly extends a sequence that previously went UP. So, dec = inc + 1.
 * - If elements are equal, the turbulence is broken. Both reset to 1.
 * 4. We keep a running maximum of the longest chain seen so far.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int maxTurbulenceSize(int[] arr) {
        if (arr == null || arr.length == 0) return 0;
        
        // Base case: A single element is trivially a valid turbulent subarray of length 1
        int inc = 1;
        int dec = 1;
        int maxLen = 1;
        
        for (int i = 1; i < arr.length; i++) {
            
            if (arr[i] > arr[i - 1]) {
                // We went UP. This perfectly extends a chain that just went DOWN.
                inc = dec + 1;
                dec = 1; // The sequence ending in DOWN is broken, reset it.
            } 
            else if (arr[i] < arr[i - 1]) {
                // We went DOWN. This perfectly extends a chain that just went UP.
                dec = inc + 1;
                inc = 1; // The sequence ending in UP is broken, reset it.
            } 
            else {
                // Flatline (arr[i] == arr[i-1]). The zigzag pattern is completely broken.
                inc = 1;
                dec = 1;
            }
            
            // The global maximum could come from either ending on an UP or a DOWN step
            maxLen = Math.max(maxLen, Math.max(inc, dec));
        }
        
        return maxLen;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N) (A single pass through the array).
 * Space: O(1) (Only tracking three scalar variables instead of full DP arrays).
 */
````