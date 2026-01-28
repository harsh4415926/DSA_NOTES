# previous smaller element
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: Nearest Smaller Element (Previous Smaller Element)
 * ----------------------------------------------------------------------
 * Find the nearest element to the LEFT that is strictly smaller than the current element.
 * * STEPS:
 * 1. Traverse Left to Right -> Pop stack elements >= current (we need smaller).
 * 2. If stack empty -> -1, else result is stack top -> Push current element.
 * ----------------------------------------------------------------------
 */

class Solution {
    public static ArrayList<Integer> prevSmaller(int[] arr) {
        int n = arr.length;
        Stack<Integer> st = new Stack<>();
        ArrayList<Integer> ans = new ArrayList<>();
        
        // Traverse from Left to Right
        for (int i = 0; i < n; i++) {
            
            // Remove larger/equal elements; they can't be the "smaller" previous for this or future
            while (st.size() > 0 && st.peek() >= arr[i]) {
                st.pop();
            }
            
            // If stack empty, no smaller element exists on the left
            if (st.size() == 0) {
                ans.add(-1);
            } 
            // Else, top is the nearest smaller element
            else {
                ans.add(st.peek());
            }
            
            // Push current element for future right neighbors
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
 * - Standard monotonic stack logic; each element pushed/popped at most once.
 * * Space Complexity: O(N)
 * - Stack stores elements in increasing order.
 * ----------------------------------------------------------------------
 */
````