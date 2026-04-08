# sum of subarray ranges
### just return sum of subarray maximums - sum of subarray minimums

````java

/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 2104 - Sum of Subarray Ranges
 * ----------------------------------------------------------------------
 * LOGIC (Monotonic Stack + Combinatorics):
 * 1. Mathematical Trick: The sum of all subarray ranges is equal to:
 * (Total Sum of Subarray Maximums) - (Total Sum of Subarray Minimums).
 * 2. To find the Sum of Minimums efficiently: For every element arr[i], we
 * want to know exactly how many subarrays it acts as the absolute minimum.
 * 3. We use a Monotonic Stack to find:
 * - NSE (Next Smaller Element): The boundary on the right.
 * - PSE (Previous Smaller Element): The boundary on the left.
 * 4. Combinatorics: If an element is the minimum for 'L' elements to its
 * left and 'R' elements to its right, it is the minimum for exactly
 * (L * R) total subarrays. We multiply this count by arr[i].
 * 5. DUPLICATE HANDLING: Notice how you used `>=` for the right boundary
 * and `>` for the left boundary. This asymmetrical strictness is the
 * secret to preventing double-counting when identical numbers exist!
 * ----------------------------------------------------------------------
 */

import java.util.Stack;

class Solution {
    public long subArrayRanges(int[] nums) {
        // Range Sum = Sum(Maxes) - Sum(Mins)
        return sumSubarrayMaximums(nums) - sumSubarrayMinimums(nums);
    }

    public long sumSubarrayMinimums(int[] arr) {
        int n = arr.length;
        int[] nse = new int[n];
        int[] pse = new int[n];
        Stack<Integer> st = new Stack<>();

        // Pass 1: Find Next Smaller Element (NSE)
        for(int i = n - 1; i >= 0; i--) {
            // Keep stack strictly increasing. Pop elements greater than or EQUAL to current.
            while(st.size() > 0 && arr[st.peek()] >= arr[i]) st.pop();
            // If stack is empty, there is no smaller element to the right. Boundary is 'n'.
            if(st.size() == 0) nse[i] = n;
            else nse[i] = st.peek();
            st.push(i);
        }

        st.clear();

        // Pass 2: Find Previous Smaller Element (PSE)
        for(int i = 0; i < n; i++) {
            // Pop elements strictly GREATER than current. (Keeps equal elements to prevent double-counting).
            while(st.size() > 0 && arr[st.peek()] > arr[i]) st.pop();
            // If stack is empty, there is no smaller element to the left. Boundary is '-1'.
            if(st.size() == 0) pse[i] = -1;
            else pse[i] = st.peek();
            st.push(i);
        }

        long ans = 0;
        // Pass 3: Calculate total contribution of each element as a minimum
        for(int i = 0; i < n; i++) {
            int leftCovered = i - pse[i];
            int rightCovered = nse[i] - i;
            ans = ans + ((long)leftCovered * rightCovered) * arr[i];
        }
        return ans;
    }

    public long sumSubarrayMaximums(int[] arr) {
        int n = arr.length;
        int[] nge = new int[n];
        int[] pge = new int[n];
        Stack<Integer> st = new Stack<>();

        // Pass 1: Find Next Greater Element (NGE)
        for(int i = n - 1; i >= 0; i--) {
            while(st.size() > 0 && arr[st.peek()] <= arr[i]) st.pop();
            if(st.size() == 0) nge[i] = n;
            else nge[i] = st.peek();
            st.push(i);
        }

        st.clear();

        // Pass 2: Find Previous Greater Element (PGE)
        for(int i = 0; i < n; i++) {
            while(st.size() > 0 && arr[st.peek()] < arr[i]) st.pop();
            if(st.size() == 0) pge[i] = -1;
            else pge[i] = st.peek();
            st.push(i);
        }

        long ans = 0;
        // Pass 3: Calculate total contribution of each element as a maximum
        for(int i = 0; i < n; i++) {
            int leftCovered = i - pge[i];
            int rightCovered = nge[i] - i;
            ans = ans + ((long)leftCovered * rightCovered) * arr[i];
        }
        return ans;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N). Each element is pushed and popped from the stack at most once.
 * Space: O(N). For the NGE, PGE, NSE, PSE arrays and the Stack.
 */
````