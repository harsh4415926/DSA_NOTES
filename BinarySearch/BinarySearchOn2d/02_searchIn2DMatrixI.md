# search in a 2d matrix 1(leetcode)
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 74 - Search a 2D Matrix
 * ----------------------------------------------------------------------
 * LOGIC (Flattened Binary Search):
 * 1. Because the matrix is strictly sorted (last element of a row is 
 * smaller than the first element of the next row), we can logically 
 * treat the entire 2D matrix as a single 1D sorted array.
 * 2. Search Space: 0 to (Rows * Cols) - 1.
 * 3. Crucial Math: To map a 1D index 'mid' back to 2D coordinates:
 * - Row = mid / (Number of Columns)
 * - Col = mid % (Number of Columns)
 * 4. Perform a standard binary search using these mapped coordinates.
 * ----------------------------------------------------------------------
 */

class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length;       // Number of rows
        int n = matrix[0].length;    // Number of columns
        
        int low = 0;
        int high = m * n - 1;        // Logically flattened array bounds

        while (low <= high) {
            int mid = low + (high - low) / 2;

            // Map the 1D 'mid' index back to 2D matrix coordinates
            int row = mid / n;
            int col = mid % n;

            // Standard binary search comparisons
            if (matrix[row][col] == target) {
                return true;
            } 
            else if (target < matrix[row][col]) {
                high = mid - 1;      // Target is smaller, search left half
            } 
            else {
                low = mid + 1;       // Target is larger, search right half
            } 
        }
        
        return false; // Target not found
    }
}

/*
 * COMPLEXITY:
 * Time: O(log(M * N)) where M is rows and N is columns. This is perfectly optimal.
 * Space: O(1) tracking only pointers.
 */
````
