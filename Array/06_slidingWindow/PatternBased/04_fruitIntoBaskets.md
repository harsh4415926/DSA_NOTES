# fruit into baskets
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 904 - Fruit Into Baskets
 * ----------------------------------------------------------------------
 * Find the longest subarray containing at most 2 unique types of fruits.
 * * STEPS:
 * 1. Expand 'right', add fruit type to frequency map.
 * 2. If map size > 2 (invalid) -> Shrink 'left' (remove counts) until valid -> Update max.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int totalFruit(int[] fruits) {
        HashMap<Integer, Integer> map = new HashMap<>();
        int left = 0;
        int maxLen = 0;
        
        for (int right = 0; right < fruits.length; right++) {
            // Add current fruit type to the basket
            map.put(fruits[right], map.getOrDefault(fruits[right], 0) + 1);

            // If we have more than 2 types, shrink window from left
            while (map.size() > 2) {
                map.put(fruits[left], map.get(fruits[left]) - 1);
                
                // If count becomes 0, remove the key to reduce map size
                if (map.get(fruits[left]) == 0) {
                    map.remove(fruits[left]);
                }
                left++;
            }
            
            // Update the maximum length of the valid window
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
 * - Each fruit is added to the map once and removed at most once.
 * * Space Complexity: O(1)
 * - The map contains at most 3 entries at any time (since we shrink at >2).
 * ----------------------------------------------------------------------
 */
````