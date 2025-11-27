# Problem: Subarray XOR Equals K(gfg)
You are given an array of integers arr and an integer k. Your task is to find the total number of continuous subarrays whose elements, when XORed together, produce a result exactly equal to k.

This problem can be solved efficiently using a HashMap to store the frequencies of prefix XORs, similar to the "Subarray Sum Equals K" problem.

Example: Input: arr = [4, 2, 2, 6, 4], k = 6 Output: 4 Explanation: The subarrays with XOR 6 are [4, 2], [4, 2, 2, 6], [2, 2, 6], and [6].

---
````java
class Solution {
    /**
     * Finds the total number of subarrays whose elements XOR to k.
     * @param arr The input array of integers.
     * @param k The target XOR value.
     * @return The total count of such subarrays.
     */
    public long subarrayXor(int arr[], int k) {
        // code here
        int n = arr.length;
        // 'xor' will store the running prefix XOR (from index 0 to i)
        int xor = 0;
        
        // HashMap stores (prefix_XOR, frequency_of_that_XOR)
        HashMap<Integer, Integer> map = new HashMap<>();
        
        // Initialize the map with XOR=0 and frequency=1.
        // This handles cases where a subarray *starting from index 0* has an XOR of k.
        map.put(0, 1);
        
        int count = 0;
        
        for (int i = 0; i < n; i++) {
            // Update the running prefix XOR
            xor ^= arr[i];
            
            // Let the current prefix XOR be 'xor' (XOR from 0 to i).
            // We are looking for a prefix XOR 'x' (XOR from 0 to j, where j < i)
            // such that: x ^ (subarray_xor) = xor
            // We want subarray_xor = k, so:
            // x ^ k = xor
            // x = xor ^ k
            // 'required' is the prefix XOR we need to find in our map.
            int required = xor ^ k;
            
            // If 'required' exists in the map, it means we found subarrays ending at 'i'
            // whose XOR is k. Add their frequency to the count.
            count += map.getOrDefault(required, 0);
            
            // Add the current prefix XOR 'xor' to the map, updating its frequency.
            map.put(xor, map.getOrDefault(xor, 0) + 1);
        }
        return count;

        // Time Complexity: O(N)
        // - We iterate through the array once.
        // - HashMap 'put' and 'get' operations are O(1) on average.

        // Space Complexity: O(N)
        // - In the worst case, the HashMap could store up to N distinct prefix XORs.
    }
}

````
