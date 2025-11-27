# Problem: Edit Distance(leetcode)
Given two strings, word1 and word2, you need to find the minimum number of operations required to convert word1 into word2. The allowed operations are:

Insert a character

Delete a character

Replace a character

Example:

Input: word1 = "horse", word2 = "ros"

Output: 3

Explanation:

horse -> rorse (replace 'h' with 'r')

rorse -> rose (delete 'r')

rose -> ros (delete 'e')

---

## memoization
````java
class Solution {
    // helper(word1, word2, index_i, index_j, dp)
    public int helper(String word1, String word2, int i, int j, int[][] dp) {
        // Base case: If word2 is empty, we must delete all remaining chars of word1.
        if (j < 0) {
            return i + 1;
        }
        // Base case: If word1 is empty, we must insert all remaining chars of word2.
        if (i < 0) {
            return j + 1;
        }
        
        // Return the cached result if available.
        if (dp[i][j] != -1) return dp[i][j];

        // If the characters match, no operation is needed.
        if (word1.charAt(i) == word2.charAt(j)) {
            return dp[i][j] = helper(word1, word2, i - 1, j - 1, dp);
        }
        // If characters don't match, find the minimum of the 3 operations.
        else {
            // Cost of replacing the character.
            int replace = 1 + helper(word1, word2, i - 1, j - 1, dp);
            // Cost of deleting the character from word1.
            int delete = 1 + helper(word1, word2, i - 1, j, dp);
            // Cost of inserting the character (equivalent to deleting from word2).
            int add = 1 + helper(word1, word2, i, j - 1, dp);
            
            return dp[i][j] = Math.min(replace, Math.min(delete, add));
        }
    }

    public int minDistance(String word1, String word2) {
        int n1 = word1.length();
        int n2 = word2.length();
        int[][] dp = new int[n1][n2];
        for (int i = 0; i < n1; i++) Arrays.fill(dp[i], -1);
        
        // Start recursion from the end of both strings.
        return helper(word1, word2, n1 - 1, n2 - 1, dp);
    }

    // Time Complexity: O(N * M)
    // - Where N and M are the lengths of the two strings.
    // - Each state (i, j) is computed once.

    // Space Complexity: O(N * M)
    // - For the DP table and the recursion stack.
}
````
## tabulation
````java
class Solution {
    public int minDistance(String word1, String word2) {
         int n1=word1.length();
       int n2=word2.length();
       int[][] dp=new int[n1+1][n2+1];
        for(int i=0;i<n1+1;i++)dp[i][0]=i; // (i-1)+1  since in the base case iand jare indices of the string so to convert indices of string here i -> i-1 , similarily for j
        for(int j=1;j<n2+1;j++)dp[0][j]=j; // (j-1)+1
        for(int i=1;i<n1+1;i++){
           for(int j=1;j<n2+1;j++){
            if(word1.charAt(i-1)==word2.charAt(j-1)){
           dp[i][j]=dp[i-1][j-1];
        }
        else{
            int replace=1+dp[i-1][j-1];
            int delete=1+dp[i-1][j];
            int add=1+dp[i][j-1];
             dp[i][j]=Math.min(replace,Math.min(delete,add));

        }
           }
        }

return dp[n1][n2];
    }
}
````
