````java
/*
* ----------------------------------------------------------------------
* PROBLEM: LeetCode 1191 - K-Concatenation Maximum Sum
* ----------------------------------------------------------------------
* LOGIC (Kadane's Algorithm + Math):
* 1. The problem asks for the max subarray sum if the array is repeated 'k' times.
* (Note: An empty subarray has a sum of 0, so if all numbers are negative, the answer is 0).
* 2. Case 1: k == 1. This is just standard Kadane's algorithm on the given array.
* 3. Case 2: k > 1. We must look at the total sum of the single array:
* - If totalSum <= 0: The sum won't grow if we span across multiple full arrays.
* The maximum sum will either be entirely inside one array, or span exactly across
* the boundary of two arrays (Max Suffix + Max Prefix). Both of these scenarios
* are perfectly captured by running Kadane's on the array concatenated TWICE.
* - If totalSum > 0: The sum WILL grow. We want to take the max boundary crossing
* (which is Kadane's on array repeated twice) AND add the total sum of the
* remaining (k - 2) full arrays sandwiched in the middle.
* 4. We must use 'long' for intermediate calculations to prevent overflow, then modulo
* 10^9 + 7 at the very end.
* ----------------------------------------------------------------------
*/

class Solution {

    // Helper function to run Kadane's Algorithm on the array repeated 'repeat' times
    private long getKadane(int[] arr, int repeat) {
        long maxSoFar = 0; // Starts at 0 because an empty subarray is allowed
        long currentMax = 0;
        
        for (int i = 0; i < arr.length * repeat; i++) {
            // Modulo arithmetic to simulate the array repeating without actually building it
            int num = arr[i % arr.length]; 
            
            // Standard Kadane's step: extend the sum, or reset to 0 if it goes negative
            currentMax = Math.max(0, currentMax + num);
            maxSoFar = Math.max(maxSoFar, currentMax);
        }
        
        return maxSoFar;
    }

    public int kConcatenationMaxSum(int[] arr, int k) {
        int MOD = 1_000_000_007;
        
        // Step 1: Calculate the total sum of a single array
        long totalSum = 0;
        for (int num : arr) {
            totalSum += num;
        }

        // Step 2: If k == 1, just return standard Kadane's
        if (k == 1) {
            return (int) (getKadane(arr, 1) % MOD);
        }

        // Step 3: Calculate Kadane's for 2 concatenated arrays (handles boundary overlaps)
        long kadane2 = getKadane(arr, 2);

        // Step 4: Apply the logic based on totalSum
        if (totalSum > 0) {
            // Add the overlapping boundary max + all the full array sums in the middle
            long result = kadane2 + (k - 2) * totalSum;
            return (int) (result % MOD);
        } else {
            // The total sum is a liability, so just return the best 2-array overlap
            return (int) (kadane2 % MOD);
        }
    }
}

/*
* COMPLEXITY:
* Time: O(N) where N is the length of the array. We iterate through the array at most 3 times.
* Space: O(1) as we simulate the concatenation using modulo math instead of creating new arrays.
  */
````