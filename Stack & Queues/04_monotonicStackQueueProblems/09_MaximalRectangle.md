# maximal rectangle
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 85 - Maximal Rectangle
 * ----------------------------------------------------------------------
 * Find the largest rectangle containing only 1s in a binary matrix.
 * * STEPS:
 * 1. Convert each row into a histogram based on column heights (cumulative 1s).
 * 2. If '0', height resets to 0. If '1', height = prev_row_height + 1.
 * 3. Calculate Largest Rectangle in Histogram for each row and maximize answer.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int maximalRectangle(char[][] matrix) {
        if (matrix.length == 0) return 0;
        int m = matrix.length;
        int n = matrix[0].length;
        
        // This stores the height of consecutive 1s ending at row i
        int[][] prefixSum = new int[m][n];

        // 1. Build the histogram heights for each cell
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == '1') {
                    // If current is 1, add to the height accumulated from above
                    // If it's the first row, height is just 1
                    prefixSum[i][j] = (i - 1) >= 0 ? (prefixSum[i - 1][j] + 1) : 1;
                } else {
                    // If '0', the consecutive sequence breaks, height resets to 0
                    prefixSum[i][j] = 0;
                }
            }
        }

        int ans = 0;
        // 2. Compute max area for each row's histogram
        for (int i = 0; i < m; i++) {
            ans = Math.max(ans, largestRectangleInHistogram(prefixSum[i]));
        }
        return ans;
    }

    // Helper: Monotonic Stack to find max area in a histogram array (O(N))
    public int largestRectangleInHistogram(int[] heights) {
        int n = heights.length;
        Stack<Integer> st = new Stack<>();
        int maxArea = 0;

        // Iterate up to n (inclusive) to handle remaining elements in stack
        for (int i = 0; i <= n; i++) {
            // When i == n, we treat height as 0 to force popping all remaining elements
            int currHeight = (i == n) ? 0 : heights[i];

            // If current bar is lower than stack top, we found a right boundary
            while (!st.isEmpty() && heights[st.peek()] > currHeight) {
                int height = heights[st.pop()];
                int right = i; // Right boundary (exclusive)
                int left = st.isEmpty() ? -1 : st.peek(); // Left boundary (exclusive)
                
                int width = right - left - 1;
                maxArea = Math.max(maxArea, height * width);
            }

            st.push(i);
        }

        return maxArea;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(M * N)
 * - M rows, N columns. We iterate over the matrix to build heights (M*N).
 * - For each row, we run histogram logic which takes O(N). Total: M * O(N).
 * * Space Complexity: O(M * N)
 * - Used for the prefixSum matrix.
 * - (Optimization: Can be reduced to O(N) by using a single 1D array for heights).
 * ----------------------------------------------------------------------
 */
````