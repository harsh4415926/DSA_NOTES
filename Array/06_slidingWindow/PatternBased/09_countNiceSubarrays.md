# count nice subarrays(similar to previous)
## using sliding window
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 1248 - Count Number of Nice Subarrays
 * ----------------------------------------------------------------------
 * Count subarrays containing exactly 'k' odd numbers.
 * * STRATEGY:
 * This is mathematically identical to "Binary Subarrays With Sum" (LeetCode 930) 
 * if we treat odd numbers as 1 and even numbers as 0.
 * Method: Count(Exact k) = Count(At Most k) - Count(At Most k-1).
 * ----------------------------------------------------------------------
 */

class Solution {
    public int numberOfSubarrays(int[] nums, int k) {
        // "Exactly k" odds = "At most k" odds - "At most k-1" odds
        return helper(nums, k) - helper(nums, k - 1);
    }
    
    // Helper: Standard Sliding Window to count subarrays with AT MOST k odd numbers
    public int helper(int[] nums, int k) {
        if (k < 0) return 0;
        int n = nums.length;
        int left = 0;
        int currCount = 0; // Counts odd numbers in current window
        int ans = 0;
        
        for (int right = 0; right < n; right++) {
            // If current number is odd, increment count
            if (nums[right] % 2 != 0) currCount++;
            
            // Shrink window if we have more than k odd numbers
            while (currCount > k) {
                if (nums[left] % 2 != 0) currCount--;
                left++;
            }
            
            // Add all valid subarrays ending at 'right'
            // For a window [left...right] with <= k odds, any subarray starting 
            // between left and right (inclusive) and ending at right is also valid.
            ans += (right - left + 1);
        }
        return ans;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N)
 * - Two passes (one for k, one for k-1), each O(N).
 * * Space Complexity: O(1)
 * - No extra data structures used.
 * ----------------------------------------------------------------------
 */
````
## using hashMap(prefixSum)
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 1248 - Count Number of Nice Subarrays (HashMap)
 * ----------------------------------------------------------------------
 * Count subarrays containing exactly 'k' odd numbers.
 * * STRATEGY:
 * Treat odd numbers as 1 and even numbers as 0. This transforms the problem 
 * into "Find number of subarrays with Sum = k" (similar to LeetCode 560).
 * Use Prefix Sums + HashMap to solve in O(N).
 * ----------------------------------------------------------------------
 */

class Solution {
    public int numberOfSubarrays(int[] nums, int k) {
        int n = nums.length;
        HashMap<Integer, Integer> map = new HashMap<>();
        
        // Base case: 0 odd numbers seen initially (handles subarrays starting at index 0)
        map.put(0, 1);
        
        int ans = 0;
        int currCount = 0; // Running count of odd numbers (Prefix Sum)
        
        for (int i = 0; i < n; i++) {
            // Treat odd as 1, even as 0
            if (nums[i] % 2 != 0) currCount++;
            
            // Check if there exists a previous prefix state such that:
            // (Current Odds) - (Old Odds) = k
            int count = map.getOrDefault(currCount - k, 0);
            ans += count;
            
            // Record current prefix sum frequency
            map.put(currCount, map.getOrDefault(currCount, 0) + 1);
        }
        return ans;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N)
 * - Single pass. HashMap operations take O(1) on average.
 * * Space Complexity: O(N)
 * - Map stores at most N distinct counts in the worst case (e.g., all odds).
 * ----------------------------------------------------------------------
 */
````