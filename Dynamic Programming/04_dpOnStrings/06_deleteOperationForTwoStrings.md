# Problem: Delete Operation for Two Strings
Given two strings, word1 and word2, your goal is to find the minimum number of deletions required to make both strings identical. You can delete characters from either word1 or word2.

The Key Insight:
The final string, after all deletions, will be the Longest Common Subsequence of the original two strings. Therefore, the total number of characters you need to delete is the sum of the lengths of both strings minus twice the length of their LCS.

Example:

Input: word1 = "sea", word2 = "eat"

LCS("sea", "eat"): "ea" (length 2)

Deletions from "sea": length("sea") - length("ea") = 3 - 2 = 1 (delete 's')

Deletions from "eat": length("eat") - length("ea") = 3 - 2 = 1 (delete 't')

Total Deletions: 1 + 1 = 2

Formula: (3 + 3) - 2 * 2 = 6 - 4 = 2

---
## solution
````java
 public int minDistance(String word1, String word2) {
        return (word1.length()+word2.length())-2*longestCommonSubsequence(word1,word2);
    }
````
