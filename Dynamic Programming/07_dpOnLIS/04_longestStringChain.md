# Problem: Longest String Chain
You are given an array of words. A word chain is a sequence of words [word_1, word_2, ..., word_k] where word_{i+1} is formed by adding exactly one letter anywhere in word_i to make them equal. Your goal is to find the length of the longest possible word chain.

Example:

Input: words = ["a", "b", "ba", "bca", "bda", "bdca"]

Output: 4

Explanation: One of the longest word chains is ["a", "ba", "bda", "bdca"].

---
class Solution {
// Helper function to check if s1 is a predecessor of s2 in a chain.
public boolean compare(String s1, String s2) {
// The second string must be exactly one character longer.
if (s1.length() + 1 != s2.length()) return false;

        int i = 0; // Pointer for s1
        int j = 0; // Pointer for s2
        
        // Iterate through both strings to find if there is at most one difference.
        while (j < s2.length()) {
            if (i < s1.length() && s1.charAt(i) == s2.charAt(j)) {
                i++;
                j++;
            } else {
                j++; // If chars don't match, only move the pointer for the longer string.
            }
        }
        // If we've traversed both strings with at most one difference, it's a valid pair.
        return i == s1.length() && j == s2.length();
    }

    public int longestStrChain(String[] words) {
        int n = words.length;
        // dp[i] stores the length of the longest chain ending with words[i].
        int[] dp = new int[n];
        // Crucial first step: sort the words by their length.
        Arrays.sort(words, (a, b) -> a.length() - b.length());
        Arrays.fill(dp, 1);
        
        int max = 1;
        // Use the LIS pattern to find the longest chain.
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                // If words[j] is a predecessor of words[i] and forms a longer chain...
                if (compare(words[j], words[i]) && 1 + dp[j] > dp[i]) {
                    // ...update the longest chain length for words[i].
                    dp[i] = 1 + dp[j];
                }
            }
            // Track the overall maximum chain length found so far.
            max = Math.max(max, dp[i]);
        }
        return max;
    }

    // Time Complexity: O(N^2 * L)
    // - Where N is the number of words and L is the max length of a word.
    // - The nested loops run in O(N^2), and each comparison takes O(L).
    // - The initial sort takes O(N log N).

    // Space Complexity: O(N)
    // - For the 'dp' array.
}
````