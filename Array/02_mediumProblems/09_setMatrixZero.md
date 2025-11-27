# Problem: Set Matrix Zeroes
You are given an m x n integer matrix. If any element in the matrix is 0, your task is to set its entire row and entire column to 0. This modification must be done in-place.

Example: Input:

[
[1, 1, 1],
[1, 0, 1],
[1, 1, 1]
]
Output:
Set Matrix Zeroes

[
[1, 0, 1],
[0, 0, 0],
[1, 0, 1]
]

---
## using separate data structure
````java
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        
        // Use two arrays to mark which rows and columns need to be zeroed.
        int[] markRowZero = new int[m];
        int[] markColZero = new int[n];
        
        // First pass: Find all zeros and mark their respective rows and columns.
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    markRowZero[i] = 1;
                    markColZero[j] = 1;
                }
            }
        }
        
        // Second pass: Use the marked arrays to set elements to zero.
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // If either the row or the column was marked, set the element to 0.
                if (markRowZero[i] == 1 || markColZero[j] == 1) {
                    matrix[i][j] = 0;
                }
            }
        }

        // Time Complexity: O(m * n)
        // - We iterate through the (m * n) matrix twice.
        // - O(m * n) + O(m * n) = O(m * n).

        // Space Complexity: O(m + n)
        // - We use two extra arrays: 'markRowZero' of size 'm' and 'markColZero' of size 'n'.
    }
}
````
## using the given matrix itself, no extra space
````java
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        
        // Use the first row and first column as markers, but we need
        // separate flags for them since matrix[0][0] overlaps.
        boolean isfirstRow = false; // Flag for the first row
        boolean isfirstCol = false; // Flag for the first column
        
        // 1. Check if the original first column has any zeros
        for (int i = 0; i < m; i++) {
            if (matrix[i][0] == 0) {
                isfirstCol = true;
                break;
            }
        }
        
        // 2. Check if the original first row has any zeros
        for (int j = 0; j < n; j++) {
            if (matrix[0][j] == 0) {
                isfirstRow = true;
                break;
            }
        }
        
        // 3. Use the first row/col to mark zeros in the *inner* matrix (from [1,1])
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0; // Mark the row
                    matrix[0][j] = 0; // Mark the column
                }
            }
        }
        
        // 4. Set zeros for the inner matrix based on the markers
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }
        
        // 5. Handle the first row (if it was originally marked)
        if (isfirstRow) {
            for (int j = 0; j < n; j++) matrix[0][j] = 0;
        }
        
        // 6. Handle the first column (if it was originally marked)
        if (isfirstCol) {
            for (int i = 0; i < m; i++) matrix[i][0] = 0;
        }

        // Time Complexity: O(m * n)
        // - We iterate through the matrix a constant number of times.
        // - (m) + (n) + (m*n) + (m*n) + (n) + (m) = O(m*n).

        // Space Complexity: O(1)
        // - We only use a few extra boolean variables, not extra arrays.
        // - This is the most optimal space solution.
    }
}
````


