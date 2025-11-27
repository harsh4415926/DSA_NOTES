# Problem: Longest Increasing Subsequence(leetcode)
Given an integer array nums, your task is to find the length of the longest strictly increasing subsequence. A subsequence is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements.

Example:

Input: nums = [10, 9, 2, 5, 3, 7, 101, 18]

Output: 4

Explanation: The longest increasing subsequence is [2, 3, 7, 101], and its length is 4.

---
## memoization
````java
class Solution {
    // helper(currentIndex, previousIndex, nums, dp)
    public int helper(int i, int lastTook, int[] nums, int[][] dp) {
        int n = nums.length;
        // Base case: If we've considered all elements.
        if (i == n) return 0;
        
        // Return cached result. We use lastTook+1 to map the -1 case to index 0.
        if (dp[i][lastTook + 1] != -1) return dp[i][lastTook + 1];

        // Option 1: Take the current element nums[i].
        int take = Integer.MIN_VALUE;
        // This is only possible if it's the first element or larger than the previous one.
        if (lastTook == -1 || nums[i] > nums[lastTook]) {
            // Length is 1 + LIS of the rest. The new 'lastTook' is the current index 'i'.
            take = 1 + helper(i + 1, i, nums, dp);
        }
        
        // Option 2: Don't take the current element nums[i].
        // LIS of the rest, 'lastTook' remains unchanged.
        int notTake = helper(i + 1, lastTook, nums, dp);
        
        // Store and return the maximum of the two options.
        return dp[i][lastTook + 1] = Math.max(take, notTake);
    }

    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        // DP state: [currentIndex][previousIndex + 1]
        int[][] dp = new int[n][n + 1];
        for (int i = 0; i < n; i++) Arrays.fill(dp[i], -1);
        
        // Start from index 0, with no previous element taken (-1).
        return helper(0, -1, nums, dp);
    }

    // Time Complexity: O(N^2)
    // - Where N is the number of elements. There are N*N states,
    // - and each is computed once due to memoization.

    // Space Complexity: O(N^2)
    // - For the N x N DP table and the O(N) recursion stack.
}
````
## tabulation
````java
class Solution {
public int lengthOfLIS(int[] nums) {
int n = nums.length;
// dp[i][j+1] = LIS length from index 'i' onwards,
// with the previous element in the LIS being at index 'j'.
int[][] dp = new int[n + 1][n + 1];

        // Iterate through the array backwards (current index 'i').
        for (int i = n - 1; i >= 0; i--) {
            // Iterate through possible previous indices 'j' (where j < i).
            for (int j = i - 1; j >= -1; j--) {
                // Option 1: Don't take nums[i].
                // The length is the LIS from the next element with the same previous.
                int len = dp[i + 1][j + 1];

                // Option 2: Take nums[i].
                // This is only possible if it's the first element or larger than nums[j].
                if (j == -1 || nums[i] > nums[j]) {
                    // Compare 'not take' with 'take' (1 + LIS from next, with 'i' as the new previous).
                    len = Math.max(len, 1 + dp[i + 1][i + 1]);
                }
                
                // Store the best possible length for this state.
                dp[i][j + 1] = len;
            }
        }
        
        // Return the result for starting at index 0 with no previous element (j=-1).
        return dp[0][-1 + 1];
    }

    // Time Complexity: O(N^2)
    // - Where N is the number of elements in nums.
    // - Due to the nested loops iterating through the DP states.

    // Space Complexity: O(N^2)
    // - For the N x N DP table.
}
````
## space optimization

````java
class Solution {
    public int lengthOfLIS(int[] nums) {
         int n=nums.length;
        int[] next=new int [n+1];
        int[] cur=new int[n+1];
        for(int i=n-1;i>=0;i--){
            for(int j=i-1;j>=-1;j--){
               int len=next[j+1];
         if(j==-1|| nums[i]>nums[j]){
            len=Math.max(len,1+next[i+1]);
            
         }
         
         cur[j+1]=len;
            }
            next=cur;
        }
        return next[-1+1];
    }
}
````
## different / must remember algorithm
````java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        // dp[i] stores the length of the LIS that *ends* at index i.
        int[] dp = new int[n]; 
        // Min LIS length is 1 (the element itself).
        Arrays.fill(dp, 1); 
        
        // The LIS might not end at the last element, so 'max' tracks the overall max.
        int max = 1;

        // Iterate through each element as a potential end of a subsequence.
        for (int i = 0; i < n; i++) {
            // Check all previous elements to see if they can extend the LIS.
            for (int j = 0; j < i; j++) {
                // If a previous element is smaller, we can extend its LIS.
                if (nums[j] < nums[i]) {
                    // Update dp[i] if we found a longer LIS ending at i.
                    dp[i] = Math.max(1 + dp[j], dp[i]);
                }
            }
            // Update the overall maximum LIS length found so far.
            max = Math.max(max, dp[i]);
        }
        return max;
    }

    // Time Complexity: O(N^2)
    // - Where N is the number of elements in nums.
    // - Due to the nested loops.

    // Space Complexity: O(N)
    // - For the 1D DP array.
}
````
## using binary search
````java
class Solution {
    // Standard binary search to find the insertion point (lower_bound).
    public int binarySearch(List<Integer> temp, int target) {
        int left = 0, right = temp.size() - 1;
        int mid = 0;
        while (left <= right) {
            mid = left + (right - left) / 2;
            if (target == temp.get(mid)) return mid;
            else if (target > temp.get(mid)) left = mid + 1;
            else right = mid - 1;
        }
        return left;
    }

    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        // 'temp' does not store the LIS itself, but a sorted list of tails
        // of potential increasing subsequences.
        List<Integer> temp = new ArrayList<>();
        temp.add(nums[0]);

        for (int i = 1; i < n; i++) {
            // If nums[i] is larger than the tail of our longest sequence,
            // we can extend it by appending nums[i].
            if (nums[i] > temp.get(temp.size() - 1)) {
                temp.add(nums[i]);
            }
            // Otherwise, find the smallest tail that is >= nums[i] and replace it.
            // This creates a new subsequence of the same length but with a smaller tail,
            // which might allow for more elements to be added later.
            else {
                // Find the position to replace using binary search.
                int idx = binarySearch(temp, nums[i]);
                temp.set(idx, nums[i]);
            }
        }

        // The size of this 'temp' list gives the length of the LIS.
        return temp.size();
    }

    // Time Complexity: O(N * log N)
    // - Where N is the number of elements.
    // - We iterate through the array once (O(N)), and each step
    // - involves a binary search on the temp list (O(log N)).

    // Space Complexity: O(N)
    // - In the worst case, the 'temp' list could store all N elements.
}
````
