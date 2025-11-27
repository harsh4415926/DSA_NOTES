# Problem: Palindrome Partitioning II
Given a string s, you need to partition it in such a way that every substring in the partition is a palindrome. Your task is to find the minimum number of cuts required for such a palindrome partitioning.

Example: Input: s = "aab" Output: 1 Explanation: The partition ["aa", "b"] can be achieved with one cut, where both "aa" and "b" are palindromes.

---
## 1st approach
````java
class Solution {
    // dpP[i][j] stores whether the substring s[i...j] is a palindrome
    int[][] dpP;

    // Helper function to check if a substring is a palindrome using memoization
    public int palindrome(String s,int i,int j){
        if(i>=j)return 1; // Base case: single char or empty string is a palindrome
        if(dpP[i][j]!=-1)return dpP[i][j]; // Return cached result
        if(s.charAt(i)!=s.charAt(j))return dpP[i][j]= 0; // Mismatched characters
        return dpP[i][j]=palindrome(s,i+1,j-1); // Recur for inner substring
    }

    // Main recursive helper to find minimum cuts
    public int helper(int idx,String s,int[] dp){
        int n=s.length();
        if(idx==n)return 0; // Base case: reached the end of the string
        if(dp[idx]!=-1)return dp[idx]; // Return cached result

        int min=Integer.MAX_VALUE;
        // Iterate through all possible end points for the current substring
        for(int i=idx;i<n;i++){
            // If the substring from idx to i is a palindrome
            if(palindrome(s,idx,i)==1){
                // Make a cut after this palindrome and recur for the rest of the string
                int count=1+helper(i+1,s,dp);
                min=Math.min(min,count); // Keep track of the minimum cuts
            }
        }
        return dp[idx]=min; // Cache and return the result
    }

    public int minCut(String s) {
        int n=s.length();
        // dp[i] stores the minimum cuts needed for the substring s[i...n-1]
        int[] dp=new int[n];
        Arrays.fill(dp,-1);
        
        dpP=new int[n][n];
        for(int i=0;i- < n; i++) Arrays.fill(dpP[i],-1);
        
        // The number of partitions is one more than the number of cuts
        return helper(0,s,dp)-1;
    }
}


````
## another approach (using mcm method) {will give tle on leetcode}
````java
class Solution {
    // Helper function to check if substring s[i...j] is a palindrome with memoization
    public int isPalindrome(int i,int j,String s,int[][] dpPal){
        if(i>=j)return 1; // Base case: single char or empty is a palindrome
        if(dpPal[i][j]!=-1)return dpPal[i][j]; // Return cached result
        if(s.charAt(i)!=s.charAt(j)) return dpPal[i][j]=0; // Mismatched characters
        
        // Recur for the inner substring
        return dpPal[i][j]=isPalindrome(i+1,j-1,s,dpPal);
    }

    // Main recursive helper to find minimum cuts in substring s[i...j]
    public int helper(int i ,int j, String s,int[][] dp,int[][] dpPal){
        if(i>j)return 0; // Base case for empty substring
    
        // If the substring from i to j is already a palindrome, no cuts are needed
        if(isPalindrome(i,j,s,dpPal)==1)return 0;
        if(dp[i][j]!=-1)return dp[i][j]; // Return cached result

        int min=Integer.MAX_VALUE;
        // Try making a cut at every possible position 'k'
        for(int k=i;k<j;k++){
            // Total partitions = 1 (for the current cut) + partitions in left part + partitions in right part
            int partitions=1+helper(i,k,s,dp,dpPal)+helper(k+1,j,s,dp,dpPal);
            min=Math.min(min,partitions); // Find the minimum cuts
        }
        return dp[i][j]=min; // Cache and return the result
    }

    public int minCut(String s) {
        int n=s.length();
        // dp[i][j] stores the min cuts for substring s[i...j]
        int[][] dp=new int[n][n];
        // dpPal[i][j] stores if substring s[i...j] is a palindrome
        int[][] dpPal=new int[n][n];
    
        for(int i=0;i<n;i++){
            Arrays.fill(dp[i],-1);
            Arrays.fill(dpPal[i],-1);
        }
        // Call the helper for the entire string
        return helper(0,n-1,s,dp,dpPal);
    }
}


````
