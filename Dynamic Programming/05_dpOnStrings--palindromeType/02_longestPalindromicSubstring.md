# longest palindromic substring(leetcode)
## brute force
````java
class Solution {
    public boolean palindrome(String s,int i,int j,int[][] dp){
        if(i>=j)return true;
        if(dp[i][j]!=-1)return dp[i][j]==1;
        if(s.charAt(i)!=s.charAt(j)){dp[i][j]=0;return false;}
        boolean x=palindrome(s,i+1,j-1,dp);
        dp[i][j]=x?1:0;
        return x;
    }
    public String longestPalindrome(String s) {
        int start=0;
        int max=0;
        int n=s.length();
        int[][] dp=new int[n+1][n+1];
        for(int i=0;i<n+1;i++)Arrays.fill(dp[i],-1);
        for(int i=0;i<s.length();i++){
            for(int j=i;j<s.length();j++){
                if(palindrome(s,i,j,dp)   && j-i+1>max){
                    start=i;
                    max=j-i+1;
                }
            }
        }
        return s.substring(start,start+max);
    }
}

````
## using blue print
````java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        // dp[i][j] = true if the substring s[i...j] is a palindrome.
        boolean[][] dp = new boolean[n][n];
        int max = 1; // Stores the length of the longest palindrome found.
        int idx = 0; // Stores the starting index of the longest palindrome.

        // Iterate over all possible lengths (L) of substrings.
        for (int L = 1; L <= n; L++) {
            // Iterate over all possible starting indices 'i'.
            for (int i = 0; i + L - 1 < n; i++) {
                int j = i + L - 1; // 'j' is the ending index.
                
                // Base case: Length 1 substrings are always palindromes.
                if (L == 1) {
                    dp[i][j] = true;
                }
                // Base case: Check length 2 substrings.
                else if (L == 2 && s.charAt(i) == s.charAt(j)) {
                    dp[i][j] = true;
                    // Update max length if this is the first length 2 palindrome.
                    if (max < 2) {
                        max = 2;
                        idx = i;
                    }
                }
                // For lengths > 2, check outer chars and the inner substring's result.
                else if (s.charAt(i) == s.charAt(j) && dp[i + 1][j - 1]) {
                    dp[i][j] = true;
                    // If this is a new longest palindrome, record its length and start index.
                    if (max < L) {
                        max = L;
                        idx = i;
                    }
                }
            }
        }
        // Extract the longest substring found using its start index and length.
        return s.substring(idx, idx + max);
    }

    // Time Complexity: O(N^2)
    // - Where N is the length of the string.
    // - We iterate through all O(N^2) substrings with two nested loops.

    // Space Complexity: O(N^2)
    // - For the N x N DP table.
}
````

## optimised
````java
class Solution {
    // Helper function to expand from a center and find the longest palindrome.
    public String expandFromCenter(int left, int right, String s) {
        int n = s.length();
        // Expand as long as pointers are in bounds and the characters match.
        while (left >= 0 && right < n && s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        }
        // After the loop, 'left' and 'right' are one position past the palindrome.
        // The start of the palindrome is at 'left + 1'.
        // The end is at 'right - 1'.
        // The length is (right - 1) - (left + 1) + 1 = right - left - 1.
        return s.substring(left + 1, right); // end index is exclusive.
    }

    public String longestPalindrome(String s) {
        int n = s.length();
        if (n <= 1) return s; // Base case for empty or single-char string.

        String ans = ""; // Stores the longest palindrome found.

        // Iterate through every character as a potential center.
        for (int i = 0; i < n; i++) {
            // Find the longest odd-length palindrome centered at 'i'.
            String odd = expandFromCenter(i, i, s);
            // Find the longest even-length palindrome centered between 'i' and 'i+1'.
            String even = expandFromCenter(i, i + 1, s);

            // Update the answer if a new longest palindrome is found.
            if (odd.length() > ans.length()) ans = odd;
            if (even.length() > ans.length()) ans = even;
        }
        return ans;
    }

    // Time Complexity: O(N^2)
    // - The main loop runs N times (for each center).
    // - 'expandFromCenter' can take up to O(N) in the worst case.
    // - Total time is O(N * N) = O(N^2).

    // Space Complexity: O(1)
    // - We only use constant extra space for pointers and the answer string.
}
````
