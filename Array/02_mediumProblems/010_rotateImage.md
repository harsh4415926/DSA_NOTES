# Problem: Rotate Image
You are given an n x n 2D integer matrix representing an image. Your task is to rotate the image by 90 degrees clockwise.

Example: Input:

[
[1, 2, 3],
[4, 5, 6],
[7, 8, 9]
]
Output:

[
[7, 4, 1],
[8, 5, 2],
[9, 6, 3]
]

---
## using extra matrix
````java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        
        // Create an auxiliary array of the same size
        int[][] arr = new int[n][n];
        
        // Loop through the original matrix
        // The core rotation logic: matrix[i][j] maps to arr[j][n-1-i]
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                arr[j][n - 1 - i] = matrix[i][j];
            }
        }
        
        // Copy all elements from the auxiliary array back to the original matrix
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                matrix[i][j] = arr[i][j];
            }
        }

        // Time Complexity: O(N^2)
        // - We iterate through all (n*n) elements to populate the new array.
        // - We iterate through all (n*n) elements again to copy them back.
        // - O(N^2) + O(N^2) = O(N^2).

        // Space Complexity: O(N^2)
        // - We use an extra auxiliary matrix 'arr' of size 'n x n'.
    }
}

````
## doing in place
````
    rotate by 90 degree = transpose + reverse rows
    rotate by 180 degree = reverse row + reverse column
    rotate by 270 degree = transpose + reverse col
````

````java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;

        // Step 1: Transposing the matrix
        // Transposing means swapping matrix[i][j] with matrix[j][i]
        // We only loop j from i+1 to avoid double-swapping
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                
                // swapping matrix[i][j] with matrix[j][i]
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
                
            }
        }

        // Step 2: Reversing each row
        // After transposing, we reverse each row to get the final 90-degree rotation
        for (int i = 0; i < n; i++) {
            int left = 0, right = n - 1;
            // Standard 2-pointer row reverse
            while (left < right) {
                int temp = matrix[i][left];
                matrix[i][left] = matrix[i][right];
                matrix[i][right] = temp;
                left++;
                right--;
            }
        }

        // Time Complexity: O(N^2)
        // - The transpose loop runs roughly (N^2 / 2) times.
        // - The row reversal loop runs (N * (N/2)) times.
        // - Total complexity is O(N^2) + O(N^2) = O(N^2).

        // Space Complexity: O(1)
        // - The rotation is done in-place, using only a few extra variables.
    }
}
````
