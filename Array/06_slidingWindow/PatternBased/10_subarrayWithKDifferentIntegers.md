# subarray with k different integers
````java
## using sliding window
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 992 - Subarrays with K Different Integers
 * ----------------------------------------------------------------------
 * Count subarrays containing exactly 'k' distinct integers.
 * * STRATEGY: "At Most" Trick
 * Count(Exactly k distinct) = Count(At Most k distinct) - Count(At Most k-1 distinct).
 * We use a sliding window to count subarrays with <= k distinct numbers.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int subarraysWithKDistinct(int[] nums, int k) {
        // Exactly K = (At Most K) - (At Most K-1)
        return helper(nums, k) - helper(nums, k - 1);
    }
    
    // Helper: Counts subarrays with AT MOST k distinct integers
    public int helper(int[] nums, int k) {
        if (k < 0) return 0;
        
        int n = nums.length;
        int left = 0;
        HashMap<Integer, Integer> map = new HashMap<>(); // Frequency Map
        int count = 0;
        
        for (int right = 0; right < n; right++) {
            // Add element to window
            map.put(nums[right], map.getOrDefault(nums[right], 0) + 1);
            
            // Shrink window if we have more than k distinct integers
            while (map.size() > k) {
                map.put(nums[left], map.get(nums[left]) - 1);
                if (map.get(nums[left]) == 0) {
                    map.remove(nums[left]); // Actually remove key to decrease map.size()
                }
                left++;
            }
            
            // Add count of valid subarrays ending at 'right'
            // [left...right] is valid, so [right], [right-1, right]... are all valid (<= k distinct)
            count += (right - left + 1);
        }
        return count;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N)
 * - Two passes (helper(k) and helper(k-1)).
 * * Space Complexity: O(N)
 * - HashMap can store up to N distinct elements in the worst case.
 * ----------------------------------------------------------------------
 */
````
## why hashMap(prefixSum ) method is not valid here?
currUnique is NOT monotonic.
Distinct count can increase and decrease depending on removals.

Prefix-sum logic only works when:

The value being accumulated is additive

And does not depend on window boundaries

But distinct count depends on the current subarray window, not the entire prefix.
