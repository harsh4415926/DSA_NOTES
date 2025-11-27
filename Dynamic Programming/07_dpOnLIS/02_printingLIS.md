# Problem: Print Longest Increasing Subsequence
Given an integer array nums, your task is to return one of the actual subsequences that is strictly increasing and has the maximum possible length.

Example:

Input: nums = [10, 9, 2, 5, 3, 7, 101, 18]

Output: [2, 3, 7, 101] (Note: [2, 5, 7, 101] is also a valid answer)

---
## code
````java
class Solution {
    public ArrayList<Integer> getLIS(int arr[]) {
        int n = arr.length;
        // dp[i] stores the length of the LIS ending at index i.
        int[] dp = new int[n];
        // hash[i] stores the index of the previous element in the LIS ending at i.
        int[] hash = new int[n];
        Arrays.fill(dp, 1);
        
        // Track the length and last index of the overall LIS.
        int max = 0;
        int maxIndex = -1;

        // Standard O(N^2) algorithm to find the length of LIS.
        for (int i = 0; i < n; i++) {
            hash[i] = i; // Initialize backpointer to the element itself.
            for (int j = 0; j < i; j++) {
                // If we found a longer LIS by including the current element...
                if (arr[j] < arr[i] && 1 + dp[j] > dp[i]) {
                    dp[i] = 1 + dp[j];
                    // ...update the backpointer to trace the path.
                    hash[i] = j;
                }
            }
            // Update the overall max LIS found so far.
            if (dp[i] > max) {
                max = dp[i];
                maxIndex = i;
            }
        }
        
        ArrayList<Integer> ans = new ArrayList<>();
        
        // Backtrack from the last element of the LIS using the hash array.
        while (hash[maxIndex] != maxIndex) {
            ans.add(arr[maxIndex]);
            maxIndex = hash[maxIndex]; // Move to the previous element in the sequence.
        }
        ans.add(arr[maxIndex]); // Add the first element of the LIS.
        
        // The LIS was built backwards, so reverse it to get the correct order.
        Collections.reverse(ans);
        return ans;
    }

    // Time Complexity: O(N^2)
    // - Where N is the number of elements.
    // - The nested loops for the DP calculation are the dominant part.

    // Space Complexity: O(N)
    // - For the 'dp' and 'hash' arrays.
}
````

