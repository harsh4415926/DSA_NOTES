# candy(leetcode)

## Greedy(n space)
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 135 - Candy
 * ----------------------------------------------------------------------
 * LOGIC (Greedy Two-Pass):
 * 1. Rule: Every child gets at least 1 candy. Higher rating neighbors get more.
 * 2. Left-to-Right Pass: Ensure children with higher ratings than their 
 * LEFT neighbor get more candies.
 * 3. Right-to-Left Pass: Ensure children with higher ratings than their 
 * RIGHT neighbor get more candies. We take the max() to preserve the 
 * validity of the first pass.
 * 4. Total Candies: Sum of the final distribution array.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int candy(int[] ratings) {
        int n = ratings.length;
        int[] count = new int[n];
        
        // Rule 1: Every child must have at least one candy
        Arrays.fill(count, 1);
        
        // Pass 1: Left to Right
        // Compare with the left neighbor
        for(int i = 1; i < n; i++) {
            if(ratings[i] > ratings[i-1]) {
                // Technically count[i] = count[i-1] + 1 is enough here 
                // since we initialized with 1s, but max is safe.
                count[i] = Math.max(count[i], count[i-1] + 1);
            }
        }

        // Pass 2: Right to Left
        // Compare with the right neighbor
        for(int i = n - 2; i >= 0; i--) {
            if(ratings[i] > ratings[i+1]) {
                // MUST use max here to avoid breaking the left-to-right rules
                count[i] = Math.max(count[i], count[i+1] + 1);
            }
        }

        // Aggregate total candies
        int ans = 0;
        for(int candies : count) {
            ans += candies;
        }
        return ans;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N) (Two linear passes).
 * Space: O(N) (One array to store candy counts).
 * Note: A Single-Pass O(1) space approach exists tracking peaks and valleys.
 */
````
## 