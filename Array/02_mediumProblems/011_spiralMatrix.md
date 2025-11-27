# Problem: Spiral Matrix
You are given an m x n integer matrix. Your task is to return all elements of the matrix in spiral order. You should start from the top-left element (matrix[0][0]) and move right, then down, then left, then up, and continue this spiral pattern until all elements have been visited.

Example: Input:

[
[1, 2, 3, 4],
[5, 6, 7, 8],
[9, 10, 11, 12]
]
Output: [1, 2, 3, 4, 8, 12, 11, 10, 9, 5, 6, 7]

---
````java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        List<Integer> ans = new ArrayList<>();
        
        // Define the boundaries of the spiral
        int left = 0, right = n - 1;
        int top = 0, bottom = m - 1;
        
        // Continue looping as long as the boundaries haven't crossed
        while (left <= right && top <= bottom) {
            
            // 1. Traverse from left to right (top row)
            for (int i = left; i <= right; i++) {
                ans.add(matrix[top][i]);
            }
            top++; // Move the top boundary down
            
            // 2. Traverse from top to bottom (right column)
            for (int i = top; i <= bottom; i++) {
                ans.add(matrix[i][right]);
            }
            right--; // Move the right boundary left
            
            // Check if boundaries have crossed (for single row/column cases)
            if (top <= bottom) {
                // 3. Traverse from right to left (bottom row)
                for (int i = right; i >= left; i--) {
                    ans.add(matrix[bottom][i]);
                }
                bottom--; // Move the bottom boundary up
            }
            
            // Check if boundaries have crossed
            if (left <= right) {
                // 4. Traverse from bottom to top (left column)
                for (int i = bottom; i >= top; i--) {
                    ans.add(matrix[i][left]);
                }
                left++; // Move the left boundary right
            }
        }
        return ans;
        
        // Time Complexity: O(m * n)
        // - We visit every element in the matrix exactly once.

        // Space Complexity: O(m * n)
        // - This is the space required to store the 'ans' list, which will
        //   contain all m*n elements.
        // - If the output list is not counted, space complexity is O(1).
    }
}
````
