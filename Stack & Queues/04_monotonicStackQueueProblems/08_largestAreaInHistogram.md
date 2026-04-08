# largest area in a histogram
## first approach (using pse and nse arrays)
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 84 - Largest Rectangle in Histogram
 * ----------------------------------------------------------------------
 * LOGIC (Monotonic Stack / Span Expansion):
 * 1. To find the largest rectangle, we can calculate the maximum possible
 * rectangle area that uses EACH bar as its shortest (bottleneck) height.
 * 2. If bar 'i' is the bottleneck, how wide can the rectangle be?
 * It can expand to the left until it hits a strictly smaller bar (PSE),
 * and it can expand to the right until it hits a strictly smaller bar (NSE).
 * 3. The total width of this rectangle is simply (NSE - PSE - 1).
 * 4. We use the Monotonic Stack twice to pre-calculate the NSE and PSE
 * arrays in O(N) time, exactly like you did in the previous problem!
 * 5. Finally, we iterate through every bar, calculate its maximum area
 * (width * heights[i]), and track the global maximum.
 * ----------------------------------------------------------------------
 */

import java.util.Stack;

class Solution {

    // Finds the index of the first strictly smaller element to the LEFT
    public void previousSmaller(int[] arr, int[] pse) {
        int n = arr.length;
        Stack<Integer> st = new Stack<>();

        for (int i = 0; i < n; i++) {
            // Pop elements that are taller than or equal to the current bar
            while (!st.isEmpty() && arr[st.peek()] >= arr[i]) {
                st.pop();
            }
            // If stack is empty, we can expand all the way to the left edge (-1)
            pse[i] = st.isEmpty() ? -1 : st.peek();
            st.push(i);
        }
    }

    // Finds the index of the first strictly smaller element to the RIGHT
    public void nextSmaller(int[] arr, int[] nse) {
        int n = arr.length;
        Stack<Integer> st = new Stack<>();

        for (int i = n - 1; i >= 0; i--) {
            // Pop elements that are taller than or equal to the current bar
            while (!st.isEmpty() && arr[st.peek()] >= arr[i]) {
                st.pop();
            }
            // If stack is empty, we can expand all the way to the right edge (n)
            nse[i] = st.isEmpty() ? n : st.peek();
            st.push(i);
        }
    }

    public int largestRectangleArea(int[] heights) {
        int n = heights.length;
        int[] pse = new int[n];
        int[] nse = new int[n];

        // Pass 1 & 2: Pre-compute boundaries in O(N) time
        previousSmaller(heights, pse);
        nextSmaller(heights, nse);

        int ans = Integer.MIN_VALUE;

        // Pass 3: Calculate the area using each bar as the minimum height
        for(int i = 0; i < n; i++) {
            // Width = (Right Boundary Index) - (Left Boundary Index) - 1
            int currentWidth = nse[i] - pse[i] - 1;
            int currArea = currentWidth * heights[i];

            ans = Math.max(currArea, ans);
        }

        return ans == Integer.MIN_VALUE ? 0 : ans;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N). We make 3 linear passes over the array. The stack operations (push/pop)
 * happen at most once per element across the entire loop, meaning the while
 * loops run in O(1) amortized time.
 * Space: O(N). We allocate three O(N) structures: the 'pse' array, the 'nse' array,
 * and the Stack itself.
 */
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