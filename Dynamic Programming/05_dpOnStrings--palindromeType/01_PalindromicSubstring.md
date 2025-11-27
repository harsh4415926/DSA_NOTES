# Problem: Palindromic Substrings
Given a string s, your task is to count the total number of palindromic substrings in it. A substring is a contiguous sequence of characters within the string.

Example:

Input: s = "abc"

Output: 3

Explanation: The palindromic substrings are "a", "b", "c".

Example 2:

Input: s = "aaa"

Output: 6

Explanation: The palindromic substrings are "a", "a", "a", "aa", "aa", "aaa".

---
## brute force 
````java
class Solution {
    // Helper function to check if the substring s[i...j] is a palindrome using memoization.
    public int isPalindrome(int i, int j, String s, int[][] dp) {
        // Base case: A single character or an empty range is always a palindrome.
        if (i >= j) return 1;
        
        // Return cached result (1 for true, 0 for false).
        if (dp[i][j] != -1) return dp[i][j];
        
        // If the outer characters don't match, it's not a palindrome.
        if (s.charAt(i) != s.charAt(j)) return dp[i][j] = 0;
        
        // If they match, check the inner substring recursively.
        return dp[i][j] = isPalindrome(i + 1, j - 1, s, dp);
    }

    public int countSubstrings(String s) {
        int n = s.length();
        // dp[i][j] stores whether s[i...j] is a palindrome.
        int[][] dp = new int[n][n];
        int count = 0;
        for (int i = 0; i < n; i++) Arrays.fill(dp[i], -1);

        // Iterate through all possible substrings.
        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                // If the substring s[i...j] is a palindrome, increment the count.
                if (isPalindrome(i, j, s, dp) == 1) {
                    count++;
                }
            }
        }
        return count;
    }

    // Time Complexity: O(N^2)
    // - We iterate through all O(N^2) substrings.
    // - The isPalindrome function runs in O(1) amortized time due to memoization.

    // Space Complexity: O(N^2)
    // - For the N x N DP table.
}
````
## must remember blue print
````java
class Solution {
    public int countSubstrings(String s) {
        int n = s.length();
        // dp[i][j] = true if the substring s[i...j] is a palindrome.
        boolean[][] dp = new boolean[n][n];
        int count = 0;
        
        // Iterate over all possible lengths of substrings (L).
        for (int L = 1; L <= n; L++) {
            // Iterate over all possible starting indices 'i'.
            for (int i = 0; (i + L - 1) < n; i++) {
                // 'j' is the ending index of the current substring.
                int j = i + L - 1;
                
                // Base case: Substrings of length 1 are always palindromes.
                if (i == j) { // length 1
                    dp[i][j] = true;
                }
                // Base case: Check substrings of length 2.
                else if (i + 1 == j) { // length 2
                    dp[i][j] = (s.charAt(i) == s.charAt(j));
                }
                // For lengths > 2, check outer chars and the inner substring's result.
                else {
                    dp[i][j] = (s.charAt(i) == s.charAt(j)) && dp[i + 1][j - 1];
                }
                
                // If the current substring s[i...j] is a palindrome, increment the count.
                if (dp[i][j]) count++;
            }
        }
        return count;
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
    // Helper function to expand from a given center (i, j) and count palindromes.
    public int expandFromCenter(int i, int j, String s) {
        int n = s.length();
        int count = 0;
        
        // Expand as long as pointers are in bounds and the characters match.
        while (i >= 0 && j < n && s.charAt(i) == s.charAt(j)) {
            count++; // Found a valid palindrome, increment count.
            i--;     // Move the left pointer inwards.
            j++;     // Move the right pointer outwards.
        }
        return count;
    }

    public int countSubstrings(String s) {
        int n = s.length();
        int count = 0;
        // Iterate through every character in the string as a potential center.
        for (int i = 0; i < n; i++) {
            // Find all odd-length palindromes (e.g., "aba") centered at 'i'.
            int odd = expandFromCenter(i, i, s);
            // Find all even-length palindromes (e.g., "abba") centered between 'i' and 'i+1'.
            int even = expandFromCenter(i, i + 1, s);
            
            // Add the counts for both types.
            count += odd + even;
        }
        return count;
    }

    // Time Complexity: O(N^2)
    // - The main loop runs N times (for each center).
    // - The 'expandFromCenter' function can take up to O(N) in the worst case.
    // - Total time is O(N * N) = O(N^2).

    // Space Complexity: O(1)
    // - We only use a few constant variables, so no extra space is needed
    // - that scales with the input size.
}
````
