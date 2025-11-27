Problem: Largest Divisible Subset
Given a set of distinct positive integers nums, your task is to find the largest subset such that for every pair of elements (a, b) in this subset, either a % b == 0 or b % a == 0. If there are multiple solutions, you can return any of them.

Example:

Input: nums = [1, 2, 4, 8]

Output: [1, 2, 4, 8]

Example 2:

Input: nums = [1, 2, 3]

Output: [1, 2] (or [1, 3])

---
## code
````java
class Solution {
public List<Integer> largestDivisibleSubset(int[] nums) {
// First, sort the array. This allows us to only check for nums[i] % nums[j].
Arrays.sort(nums);
int n = nums.length;
// dp[i] stores the size of the largest divisible subset (LDS) ending with nums[i].
int[] dp = new int[n];
// hash[i] stores the index of the previous element in the LDS ending at i.
int[] hash = new int[n];
Arrays.fill(dp, 1);

        // Track the size and last index of the overall LDS.
        int max = 0;
        int maxIndex = -1;

        // Use the LIS pattern to find the size and path of the LDS.
        for (int i = 0; i < n; i++) {
            hash[i] = i; // Initialize backpointer to the element itself.
            for (int j = 0; j < i; j++) {
                // If nums[i] is divisible by nums[j] and it creates a longer subset...
                if (nums[i] % nums[j] == 0 && 1 + dp[j] > dp[i]) {
                    dp[i] = 1 + dp[j];
                    // ...update the backpointer to trace the path.
                    hash[i] = j;
                }
            }
            // Update the overall max LDS found so far.
            if (dp[i] > max) {
                max = dp[i];
                maxIndex = i;
            }
        }
        
        List<Integer> ans = new ArrayList<>();
        
        // Backtrack from the last element of the LDS using the hash array.
        ans.add(nums[maxIndex]);
        while (hash[maxIndex] != maxIndex) {
            maxIndex = hash[maxIndex]; // Move to the previous element in the subset.
            ans.add(nums[maxIndex]);
        }
        
        // The list was built backwards, so reverse it for the final sorted order.
        Collections.reverse(ans);
        return ans;
    }

    // Time Complexity: O(N^2)
    // - The initial sort takes O(N log N).
    // - The nested loops for the DP calculation take O(N^2), which is dominant.

    // Space Complexity: O(N)
    // - For the 'dp' and 'hash' arrays.
}
````