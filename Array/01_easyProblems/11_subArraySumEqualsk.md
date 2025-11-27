# Problem: Subarray Sum Equals K
You are given an array of integers nums and an integer k. Your task is to find the total number of continuous subarrays whose elements sum up to exactly k. This includes handling positive, negative, and zero values in the array.

Example: Input: nums = [1, 1, 1], k = 2 Output: 2 Explanation: The two subarrays are [1, 1] (starting at index 0) and [1, 1] (starting at index 1).

---
## optimal(using hashing the prefix sum)
````java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int n = nums.length;
        // HashMap to store (prefixSum, count of times this prefixSum has occurred)
        HashMap<Integer, Integer> map = new HashMap<>();
        
        // Base case: A prefix sum of 0 has occurred once (an empty subarray)
        map.put(0, 1);
        
        int preSum = 0;
        int count = 0;
        
        for (int i = 0; i < n; i++) {
            // Calculate the prefix sum up to the current element
            preSum += nums[i];
            
            // Check if (preSum - k) exists in the map.
            // If it does, it means there is a subarray ending at 'i' whose sum is 'k'.
            // The value map.get(preSum - k) tells us how many such subarrays exist.
            if (map.containsKey(preSum - k)) {
                count += map.get(preSum - k);
            }
            
            // Add the current prefix sum to the map, or increment its count if it's already there.
            map.put(preSum, map.getOrDefault(preSum, 0) + 1);
        }
        return count;

        // Time Complexity: O(N)
        // - We iterate through the array once.
        // - HashMap 'put' and 'get' operations are O(1) on average.

        // Space Complexity: O(N)
        // - In the worst case, the HashMap could store N distinct prefix sums
        //   (e.g., if all elements are unique and positive).
    }
}
````
