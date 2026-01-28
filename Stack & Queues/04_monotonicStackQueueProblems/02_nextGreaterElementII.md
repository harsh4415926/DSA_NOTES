# next greater elemetn II
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 503 - Next Greater Element II
 * ----------------------------------------------------------------------
 * Find the Next Greater Element in a CIRCULAR array (search wraps around).
 * * STEPS:
 * 1. Pre-fill stack with all elements (simulating the circular wrap-around).
 * 2. Traverse right-to-left -> Pop smaller elements -> Update NGE -> Push current.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int[] nextGreaterElements(int[] nums) {
        int n = nums.length;
        int[] nge = new int[n];
        Stack<Integer> st = new Stack<>();
        
        // STEP 1: Pre-fill the stack 
        // We push elements from n-1 down to 0.
        // This ensures that when we process the last element (index n-1) in the next loop,
        // the stack already contains elements from the start of the array (0, 1...),
        // simulating the circular behavior.
        for (int i = n - 1; i >= 0; i--) {
            st.push(nums[i]);
        }

        // STEP 2: Standard NGE Logic (Right to Left)
        for (int i = n - 1; i >= 0; i--) {
            // Remove elements not greater than current (useless candidates)
            while (st.size() > 0 && st.peek() <= nums[i]) {
                st.pop();
            }
            
            // If stack not empty, top is the next greater element
            if (st.size() > 0) {
                nge[i] = st.peek();
            } else {
                nge[i] = -1;
            }
            
            // Push current element for left neighbors to use
            st.push(nums[i]);
        }

        return nge;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N)
 * - We iterate through the array twice (once to fill, once to process).
 * - Stack operations are amortized O(1).
 * * Space Complexity: O(N)
 * - Stack can store up to 2*N elements in the worst case before popping.
 * ----------------------------------------------------------------------
 */
````