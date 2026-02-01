# binary subarray with sum (same as subarray sum equals k)
## using sliding window
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 930 - Binary Subarrays With Sum
 * ----------------------------------------------------------------------
 * Find number of subarrays with sum equal to 'goal' in a binary array.
 * * LOGIC: "At Most" Trick
 * Calculating "exactly k" directly with a variable window is hard because
 * removing a 0 from the left doesn't change the sum (window can't shrink deterministically).
 * Instead, we use: Count(Exact k) = Count(At Most k) - Count(At Most k-1).
 * ----------------------------------------------------------------------
 */

class Solution {

    public int numSubarraysWithSum(int[] nums, int goal) {
        // Logic: (Subarrays with sum <= goal) - (Subarrays with sum <= goal-1)
        // This leaves us with only the subarrays where sum == goal.
        return helper(nums, goal) - helper(nums, goal - 1);
    }

    // Standard Sliding Window: Finds number of subarrays with sum <= k
    public int helper(int[] nums, int k) {
        if (k < 0) return 0; // Edge case for goal=0 -> helper(nums, -1) should return 0
        
        int n = nums.length;
        int left = 0;
        int currSum = 0;
        int count = 0;
        
        for (int right = 0; right < n; right++) {
            currSum += nums[right];
            
            // Shrink window while sum exceeds k
            while (currSum > k) {
                currSum -= nums[left];
                left++;
            }
            
            // Add number of valid subarrays ending at 'right'
            // If window [left...right] is valid, then [right], [right-1, right]...
            // are all valid. Total = window size.
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
 * - We call the helper function twice. Each call runs a sliding window O(N).
 * * Space Complexity: O(1)
 * - Only integer variables used.
 * ----------------------------------------------------------------------
 */
````
## using hashMap(prefixSum)
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 930 - Binary Subarrays With Sum (Prefix Sum)
 * ----------------------------------------------------------------------
 * Find number of subarrays with sum equal to 'goal' using Prefix Sums.
 * * STEPS:
 * 1. Maintain a running Prefix Sum. Store frequency of each sum in a Map.
 * 2. If (currSum - goal) exists in Map, it means there are subarrays ending
 * at current index with sum 'goal'. Add their count to answer.
 * 3. Works for any integer array (including negatives), not just binary.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int numSubarraysWithSum(int[] nums, int goal) {
        int n = nums.length;
        HashMap<Integer, Integer> map = new HashMap<>();
        
        // Base Case: A prefix sum of 0 exists once (before the array starts)
        // This handles cases where a subarray starts from index 0.
        map.put(0, 1); 
        
        int currSum = 0;
        int ans = 0;
        
        for (int i = 0; i < n; i++) {
            currSum += nums[i];
            
            // Check if there is a prefix subarray we can chop off 
            // such that remaining sum = goal.
            // (Current Prefix Sum) - (Old Prefix Sum) = Goal
            // => Old Prefix Sum = Current Prefix Sum - Goal
            int count = map.getOrDefault(currSum - goal, 0);
            ans += count;
            
            // Add current prefix sum to map for future subarrays
            map.put(currSum, map.getOrDefault(currSum, 0) + 1);
        }
        return ans;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N)
 * - Single pass through the array. Map operations are O(1).
 * * Space Complexity: O(N)
 * - Map stores up to N distinct prefix sum values in worst case.
 * ----------------------------------------------------------------------
 */
````