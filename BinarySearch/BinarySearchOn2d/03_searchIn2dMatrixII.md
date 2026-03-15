# search in a 2d matrix 2
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 240 - Search a 2D Matrix II
 * ----------------------------------------------------------------------
 * LOGIC (Staircase / Top-Right Search):
 * 1. Because rows are sorted left-to-right and columns top-to-bottom, 
 * we cannot flatten this matrix like the previous problem.
 * 2. Instead, we start at the Top-Right corner. Why? It acts as a perfect 
 * binary decision point:
 * - All elements to the LEFT are smaller.
 * - All elements BELOW are larger.
 * 3. Strategy:
 * - If target == current: Found it!
 * - If target < current: The target must be to the left (decrement column).
 * - If target > current: The target must be below (increment row).
 * ----------------------------------------------------------------------
 */

class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length;       // Number of rows
        int n = matrix[0].length;    // Number of columns
        
        // Start pointer at the top-right corner
        int i = 0;           
        int j = n - 1;       

        // Continue searching while the pointer is within matrix bounds
        while (i < m && j >= 0) {
            
            if (matrix[i][j] == target) {
                return true;  // Target found
            } 
            else if (target < matrix[i][j]) {
                j--;          // Target is smaller, eliminate current column and move left
            } 
            else {
                i++;          // Target is larger, eliminate current row and move down
            }
        }
        
        return false; // Traversed outside bounds, target does not exist
    }
}

/*
 * COMPLEXITY:
 * Time: O(M + N) where M is rows and N is columns. In the worst case, we traverse one full row and one full column.
 * Space: O(1) tracking only pointers.
 */
````