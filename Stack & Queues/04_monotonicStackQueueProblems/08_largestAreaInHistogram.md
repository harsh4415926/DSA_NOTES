# largest area in a histogram
## first approach (using pse and nse arrays)
````java
class Solution {
     public void previousSmaller(int[] arr,int[] pse) {
        int n = arr.length;
        
        Stack<Integer> st = new Stack<>();

        for (int i = 0; i < n; i++) {
           
            while (!st.isEmpty() && arr[st.peek()] >= arr[i]) {
                st.pop();
            }

            pse[i] = st.isEmpty() ? -1 : st.peek();
            st.push(i);
        }
       
    }
    public void nextSmaller(int[] arr,int[] nse) {
        int n = arr.length;
       
        Stack<Integer> st = new Stack<>();

        for (int i = n - 1; i >= 0; i--) {
            
            while (!st.isEmpty() && arr[st.peek()] >= arr[i]) {
                st.pop();
            }

            nse[i] = st.isEmpty() ? n : st.peek();
            st.push(i);
        }
        
    }
    public int largestRectangleArea(int[] heights) {
        int n=heights.length;
        int[] pse=new int[n];
        int[] nse=new int[n];
        previousSmaller(heights,pse);
        nextSmaller(heights,nse);
        int ans=Integer.MIN_VALUE;
        for(int i=0;i<n;i++){
           int currArea=(nse[i]-pse[i]-1)*heights[i];
           ans=Math.max(currArea,ans);
        }
        return ans;
    }
}
````
## second approach(optimal - one traversal)
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 84 - Largest Rectangle in Histogram
 * ----------------------------------------------------------------------
 * Find the largest rectangle area possible within a histogram.
 * * STEPS:
 * 1. Maintain monotonic increasing stack. If curr < top, top's boundary is found (NSE = curr).
 * 2. Pop top, Area = height * (NSE - PSE - 1). Process remaining stack with NSE = n.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int largestRectangleArea(int[] heights) {
        int n = heights.length;
        Stack<Integer> st = new Stack<>();
        int ans = 0; // Better to init with 0 for positive areas

        for (int i = 0; i < n; i++) {
            // While current bar is shorter than stack top, we found the right boundary (NSE)
            while (!st.isEmpty() && heights[st.peek()] > heights[i]) {
                int elem = heights[st.pop()]; // The height of the rectangle
                int nse = i;                  // Right boundary (exclusive)
                int pse = st.isEmpty() ? -1 : st.peek(); // Left boundary (exclusive)
                
                // Calculate width & area
                int currArea = (nse - pse - 1) * elem;
                ans = Math.max(ans, currArea);
            }
            st.push(i);
        }

        // Process remaining elements in stack (they extend to the end, so NSE = n)
        while (!st.isEmpty()) {
            int elem = heights[st.pop()];
            int nse = n;
            int pse = st.isEmpty() ? -1 : st.peek();
            
            int currArea = (nse - pse - 1) * elem;
            ans = Math.max(ans, currArea);
        }
        return ans;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N)
 * - Each element is pushed and popped exactly once.
 * * Space Complexity: O(N)
 * - Stack size can grow up to N in worst case (ascending sorted input).
 * ----------------------------------------------------------------------
 */
````