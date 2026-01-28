# next greater element 
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: Next Greater Element (NGE)
 * ----------------------------------------------------------------------
 * Find the first element to the right that is strictly greater than the current element.
 * * STEPS:
 * 1. Traverse array from right to left -> Pop stack elements <= current element.
 * 2. If stack empty -> result is -1, else result is stack top -> Push current element.
 * ----------------------------------------------------------------------
 */

class Solution {
    public ArrayList<Integer> nextLargerElement(int[] arr) {
        int n = arr.length;
        ArrayList<Integer> ans = new ArrayList<>();
        Stack<Integer> st = new Stack<>();

        // Traverse from Right to Left
        for (int i = n - 1; i >= 0; i--) {
            
            // Pop smaller elements; they can't be NGE for current or left elements
            while (st.size() > 0 && st.peek() <= arr[i]) {
                st.pop();
            }

            // If empty, no greater element exists
            if (st.size() == 0) {
                ans.addFirst(-1); 
            } 
            // Else, top is the nearest greater element
            else {
                ans.addFirst(st.peek());
            }
            
            // Push current element as a candidate for future elements
            st.push(arr[i]);
        }
        return ans;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N)
 * - Each element is pushed and popped at most once.
 * * Space Complexity: O(N)
 * - Worst case (decreasing array) stores all elements in stack.
 * ----------------------------------------------------------------------
 */
````