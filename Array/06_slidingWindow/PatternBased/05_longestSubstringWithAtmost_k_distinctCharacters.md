# longest substring with atmost k distinct characters
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: Longest Substring with At Most K Distinct Characters
 * ----------------------------------------------------------------------
 * Find the length of the longest substring containing at most k distinct characters.
 * * STEPS:
 * 1. Expand 'right' pointer -> Add char to frequency map.
 * 2. If map size > k (too many distinct chars) -> Shrink 'left' until valid -> Update max.
 * ----------------------------------------------------------------------
 */

public class Solution {

    public static int kDistinctChars(int k, String str) {
        HashMap<Character, Integer> map = new HashMap<>();
        int left = 0;
        int maxLen = 0;
        
        // Sliding Window: Expand right
        for (int right = 0; right < str.length(); right++) {
            char ch = str.charAt(right);
            map.put(ch, map.getOrDefault(ch, 0) + 1);
            
            // Shrink window if distinct characters exceed k
            while (map.size() > k) {
                char chLeft = str.charAt(left);
                map.put(chLeft, map.get(chLeft) - 1);
                
                // Remove character completely if count hits 0 (reduces map size)
                if (map.get(chLeft) == 0) map.remove(chLeft);
                
                left++;
            }
            
            // Valid window (at most k distinct), update max length
            maxLen = Math.max(maxLen, right - left + 1);
        }
        return maxLen;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N)
 * - Each character is processed at most twice (added by right, removed by left).
 * * Space Complexity: O(K)
 * - The HashMap stores at most K + 1 distinct characters.
 * ----------------------------------------------------------------------
 */
````