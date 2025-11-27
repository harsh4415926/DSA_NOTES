# Pascal's Triangle

# 1) to find an element at i th row and j th col

````java
   // ans[i][j]=(i-1) C (j-1)  {combination}
    public int comb(int n, int r) {
        // This calculates nCr using the formula:
        // (n * (n-1) * ... * (n-r+1)) / (r * (r-1) * ... * 1)
        long ans = 1; // Use long to prevent intermediate overflow
        for (int i = 0; i < r; i++) {
            ans = ans * (n - i);
            ans = ans / (i + 1);
        }
        return (int)ans;

        // Time Complexity: O(r)
        // - The loop runs 'r' times.

        // Space Complexity: O(1)
        // - Only constant extra space is used.
    }
    public int pascal(int i, int j) {
        // The element at row 'i', col 'j' is (i-1)C(j-1)
        return comb(i - 1, j - 1);
        
        // Time Complexity: O(j)
        // - Dominated by the comb(i-1, j-1) call, which runs in O(r) or O(j-1).

        // Space Complexity: O(1)
    }
}
````
## pascal's triangle II (leetcode) -to return ith row

````java
   public List<Integer> getRow(int rowIndex) {
        List<Integer> ans = new ArrayList<>();
        
        // 'val' holds the current combination value (nCr)
        long val = 1; // Starts at nC0, which is always 1
        
        // Loop calculates nC0, nC1, ..., nCr
        for (int j = 0; j <= rowIndex; j++) {
            ans.add((int)val);
            // Calculate the next combination value (nC(r+1)) from the current one (nCr)
            // nC(r+1) = nCr * (n - r) / (r + 1)
            // Here, n = rowIndex, r = j
            val = val * (rowIndex - j) / (j + 1);
        }
        return ans;
        
        // Time Complexity: O(rowIndex)
        // - The loop runs 'rowIndex' + 1 times.

        // Space Complexity: O(rowIndex)
        // - The 'ans' list stores 'rowIndex' + 1 elements.
    }
}
````
## pascal's triangle I (leetcode)- to return complete pascals triangle
````java
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> ans = new ArrayList<>();
        
        // Iterate for each row from 0 to numRows-1
        for (int i = 0; i < numRows; i++) {
            List<Integer> temp = new ArrayList<>();
            
            // Iterate for each element in the current row 'i'
            for (int j = 0; j <= i; j++) {
                // The first and last element of every row is 1
                if (j == 0 || j == i) {
                    temp.add(1);
                } else {
                    // Get the two elements from the previous row (i-1)
                    // The current element is the sum of elem[i-1][j-1] and elem[i-1][j]
                    int val = ans.get(i - 1).get(j - 1) + ans.get(i - 1).get(j);
                    temp.add(val);
                }
            }
            ans.add(temp); // Add the completed row to the final answer
        }
        return ans;

        // Time Complexity: O(numRows^2)
        // - We have a nested loop. The outer loop runs 'numRows' times.
        // - The inner loop runs 'i' times (0, 1, 2, ... numRows-1).
        // - Total operations are 1 + 2 + 3 + ... + numRows, which is (numRows * (numRows + 1)) / 2.
        // - This simplifies to O(numRows^2).

        // Space Complexity: O(numRows^2)
        // - We are storing all the elements of the triangle.
        // - The total number of elements is also 1 + 2 + ... + numRows, which is O(numRows^2).
    }
````
