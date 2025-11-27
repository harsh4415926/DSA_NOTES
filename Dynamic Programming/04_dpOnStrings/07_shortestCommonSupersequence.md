# Problem: Shortest Common Supersequence
Given two strings, s1 and s2, your task is to find the length of the shortest string that contains both s1 and s2 as subsequences.

Example:

Input: s1 = "abac", s2 = "cab"

Output: 5

Explanation: The shortest common supersequence is "cabac", which has a length of 5. This can also be calculated as len(s1) + len(s2) - len(LCS) = 4 + 3 - 2 = 5.

---
## memoization
````java
class Solution {
    // helper(i, j, ...) calculates SCS length for s1[0..i-1] and s2[0..j-1].
    public static int helper(int i, int j, String s1, String s2, int[][] dp) {
        // Base case: If s1 is empty, the SCS must be s2.
        if (i == 0) return j;
        // Base case: If s2 is empty, the SCS must be s1.
        if (j == 0) return i;
        
        // Return the cached result if available.
        if (dp[i][j] != -1) return dp[i][j];

        // If the last characters match...
        if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
            // ...they contribute 1 character to the SCS. Recurse on the rest.
            return dp[i][j] = 1 + helper(i - 1, j - 1, s1, s2, dp);
        }
        // If the last characters do not match...
        else {
            // ...find the minimum SCS by either:
            // 1. Including s1's last char and finding SCS for the rest of s1.
            // 2. Including s2's last char and finding SCS for the rest of s2.
            return dp[i][j] = 1 + Math.min(
                helper(i - 1, j, s1, s2, dp), 
                helper(i, j - 1, s1, s2, dp)
            );
        }
    }

    public static int shortestCommonSupersequence(String s1, String s2) {
        int n1 = s1.length();
        int n2 = s2.length();
        int[][] dp = new int[n1 + 1][n2 + 1];
        for (int i = 0; i < n1 + 1; i++) Arrays.fill(dp[i], -1);
        
        return helper(n1, n2, s1, s2, dp);
    }

    // Time Complexity: O(N * M)
    // - Where N and M are the lengths of the strings.
    // - Each state (N * M) is computed once.

    // Space Complexity: O(N * M)
    // - For the DP table and the recursion stack.
}
````
## tabulation
````java
class Solution {
    // Function to find length of shortest common supersequence of two strings.
    public static int shortestCommonSupersequence(String s1, String s2) {
        // Your code here
        int n1=s1.length();
        int n2=s2.length();
        int[][] dp=new int[n1+1][n2+1];
        for(int i=0;i<n1+1;i++){
            for(int j=0;j<n2+1;j++){
                if(i==0 || j==0){
                    dp[i][j]=i+j;  // if i is 0 then j and if j is 0 then i
                }
                else if(s1.charAt(i-1)==s2.charAt(j-1)){
            
                dp[i][j]= 1+dp[i-1][j-1];
        }
        else {
            dp[i][j]= Math.min(1+dp[i-1][j],1+dp[i][j-1]);
        }
            }
        }
        
    return dp[n1][n2];
    }
}
````
## using lcs
````java
return n1+n2 - lcs;
````


