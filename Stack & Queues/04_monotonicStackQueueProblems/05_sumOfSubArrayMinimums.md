# sum of subarray minimums 

## brute force
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 907 - Sum of Subarray Minimums (Brute Force)
 * ----------------------------------------------------------------------
 * Given an array of integers arr, find the sum of min(b), where b ranges
 * over every (contiguous) subarray of arr.
 * * STEPS:
 * 1. Generate all subarrays using nested loops (start 'i', end 'j').
 * 2. Maintain 'min' for current window [i...j] and add to total sum.
 * ----------------------------------------------------------------------
 */

class Solution {
    int mod = (int)1e9 + 7;

    public int sumSubarrayMins(int[] arr) {
        int n = arr.length;
        long ans = 0;

        // Outer loop: Start index of subarray
        for (int i = 0; i < n; i++) {
            int min = arr[i];
            
            // Inner loop: End index of subarray
            for (int j = i; j < n; j++) {
                // Dynamically update min for subarray arr[i...j]
                min = Math.min(min, arr[j]);
                
                // Accumulate the minimum to the answer
                ans = ((long)ans + min) % mod;
            }
        }
        return (int)ans;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N^2)
 * - Nested loops iterate through all possible subarrays.
 * - This solution typically gets Time Limit Exceeded (TLE) for N > 10^4.
 * * Space Complexity: O(1)
 * - Only simple variables used.
 * ----------------------------------------------------------------------
 */
````

## optimised (using pse and nse)

````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 907 - Sum of Subarray Minimums (Optimized)
 * ----------------------------------------------------------------------
 * Calculate the sum of min(b) for every subarray b of arr.
 * * STEPS:
 * 1. For each element, count how many subarrays use it as the minimum.
 * 2. Find distance to Previous Less Element (Left) & Next Less Element (Right).
 * 3. Total Contribution = arr[i] * LeftDistance * RightDistance.
 * ----------------------------------------------------------------------
 */
class Solution {
    int mod = (int)1e9 + 7;

    public int sumSubarrayMins(int[] arr) {
        int n = arr.length;
        int[] nse = new int[n];
        int[] pse = new int[n];
        Stack<Integer> st = new Stack<>();

        // 1. Find Next Smaller Element (NSE)
        for (int i = n - 1; i >= 0; i--) {
            while (!st.isEmpty() && arr[st.peek()] >= arr[i]) st.pop();
            
            nse[i] = st.isEmpty() ? n : st.peek(); // Boundary is n if none found
            st.push(i);
        }

        st.clear(); 

        // 2. Find Previous Smaller Element (PSE)
        for (int i = 0; i < n; i++) {
            while (!st.isEmpty() && arr[st.peek()] > arr[i]) st.pop(); // Strict > handles duplicates
            
            pse[i] = st.isEmpty() ? -1 : st.peek(); // Boundary is -1 if none found
            st.push(i);
        }
        
        // 3. Calculate Contribution
        long ans = 0;
        for (int i = 0; i < n; i++) {
            int leftLen = i - pse[i];
            int rightLen = nse[i] - i;
            
            // Total subarrays = left extension * right extension
            long contribution = (long) leftLen * rightLen % mod;
            ans = (ans + contribution * arr[i]) % mod;
        }
        return (int) ans;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N)
 * - Three linear passes (NSE, PSE, Contribution).
 * * Space Complexity: O(N)
 * - Two arrays and a stack.
 * ----------------------------------------------------------------------
 */
````