````java
/*
* ----------------------------------------------------------------------
* PROBLEM: LeetCode 53 - Maximum Subarray
* ----------------------------------------------------------------------
* LOGIC (Kadane's Algorithm):
* 1. The core idea is to calculate the maximum subarray sum ending at each position.
* 2. At every element, we make a simple choice:
* - Extend the previous subarray: Add the current element to the running sum.
* - Start fresh: If the previous running sum is negative and dragging us down
* (i.e., 'max + cur' is less than 'cur'), it's better to abandon it and start
* a new subarray exactly at the current element.
* 3. We continuously track the absolute maximum sum encountered so far ('ans').
* ----------------------------------------------------------------------
*/

class Solution {
public int maxSubArray(int[] nums) {
int n = nums.length;

        // 'max' tracks the maximum subarray sum that MUST end at the current index.
        int max = nums[0];
        
        // 'ans' tracks the global maximum subarray sum seen across the entire array.
        int ans = nums[0];
        
        for (int i = 1; i < n; i++) {
            int cur = nums[i];
            
            // The defining step of Kadane's Algorithm:
            // Do we add 'cur' to the existing sum, or just take 'cur' by itself?
            max = Math.max(max + cur, cur);
            
            // Update the global maximum if our current subarray sum is the highest seen
            ans = Math.max(ans, max);
        }
        
        return ans;
    }
}

/*
* COMPLEXITY:
* Time: O(N) (A single pass through the array is sufficient).
* Space: O(1) (Only tracking a few scalar variables, modifying 'max' in place).
  */
````