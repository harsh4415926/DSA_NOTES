# good days to rob bank(leetcode)

## Intuition and Approach
The problem requires us to check two conditions for any given day i: a non-increasing sequence of length time + 1 ending at i, and a non-decreasing sequence of length time + 1 starting at i.

Checking this for every valid day i would be inefficient (O(N * time)). We can optimize this by pre-calculating the lengths of these sequences for all days in O(N).

Prefix Array (prefix): Create an array prefix where prefix[i] stores the length of the non-increasing subarray ending at index i. We can calculate this in a single left-to-right pass. If security[i-1] >= security[i], then prefix[i] = prefix[i-1] + 1. Otherwise, the streak is broken, and prefix[i] = 1.

Suffix Array (suffix): Create an array suffix where suffix[i] stores the length of the non-decreasing subarray starting at index i. We can calculate this in a single right-to-left pass. If security[j] <= security[j+1], then suffix[j] = suffix[j+1] + 1. Otherwise, the streak is broken, and suffix[j] = 1.

Final Check: Iterate through all possible "good days" (from i = time to n - 1 - time). A day i is a good day if both conditions are met:

prefix[i] >= time + 1 (or prefix[i] > time)

suffix[i] >= time + 1 (or suffix[i] > time)

````java
class Solution {
    public List<Integer> goodDaysToRobBank(int[] security, int time) {
        int n = security.length;
        List<Integer> ans = new ArrayList<>();
        
        // If the array is too short to have 'time' days before AND after any day
        if (n <= time * 2) return ans;

        // prefix[i] = length of non-increasing subarray ending at i
        int[] prefix = new int[n];
        // suffix[i] = length of non-decreasing subarray starting at i
        int[] suffix = new int[n];
        
        // Initialize all counts to 1 (a single element is a valid sequence of 1)
        Arrays.fill(prefix, 1);
        Arrays.fill(suffix, 1);

        // Calculate prefix (non-increasing) and suffix (non-decreasing) arrays
        // This loop cleverly combines both passes
        for (int i = 1; i < n; i++) { // Note: loop goes to n-1
            // Left-to-right pass for non-increasing (prefix)
            if (security[i - 1] >= security[i]) {
                prefix[i] = prefix[i - 1] + 1;
            } else {
                prefix[i] = 1; // Reset the streak
            }
            
            // Right-to-left pass for non-decreasing (suffix)
            int j = n - 1 - i; // j goes from n-2 down to 0
            if (security[j] <= security[j + 1]) {
                suffix[j] = suffix[j + 1] + 1;
            } else {
                suffix[j] = 1; // Reset the streak
            }
        }

        // Check for good days
        // We only need to check days from 'time' to 'n-1-time'
        for (int i = time; i <= n - 1 - time; i++) {
             // We need a sequence of length (time + 1)
             // So, prefix[i] > time is equivalent to prefix[i] >= time + 1
             if (prefix[i] > time && suffix[i] > time) {
                 ans.add(i);
             }
        }
        return ans;
        
        // Time Complexity: O(N)
        // - O(N) to build the 'prefix' and 'suffix' arrays in one pass.
        // - O(N) (or O(N - 2*time)) to check for good days.
        // - Total: O(N).

        // Space Complexity: O(N)
        // - O(N) for the 'prefix' array.
        // - O(N) for the 'suffix' array.
        // - Total: O(N).
    }
}
````

