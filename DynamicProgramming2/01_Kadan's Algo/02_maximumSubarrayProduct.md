````java

/*
* ----------------------------------------------------------------------
* PROBLEM: LeetCode 152 - Maximum Product Subarray
* ----------------------------------------------------------------------
* LOGIC (Dynamic Programming / Kadane's Variation):
* 1. Unlike summation, multiplication introduces a twist: two negative
* numbers multiplied become a large positive number.
* 2. Therefore, we must track BOTH the maximum product so far AND the
* minimum (most negative) product so far.
* 3. The "Sign Flip": When we encounter a negative number, multiplying it
* by our current max makes it a small minimum, and multiplying it by our
* current min makes it a large maximum. We handle this by swapping min/max.
* 4. At each step, we decide whether to extend the existing contiguous
* subarray product or start a fresh subarray at the current element.
* ----------------------------------------------------------------------
*/

class Solution {
public int maxProduct(int[] nums) {
if (nums == null || nums.length == 0) return 0;

        int maxSoFar = nums[0];
        int minSoFar = nums[0];
        int result = nums[0];
        
        for (int i = 1; i < nums.length; i++) {
            int curr = nums[i];
            
            // The "Sign Flip": Multiplying by a negative swaps max and min boundaries
            if (curr < 0) {
                int temp = maxSoFar;
                maxSoFar = minSoFar;
                minSoFar = temp;
            }
            
            // Do we extend the previous subarray product, or start fresh here?
            // (Starting fresh happens if the current number itself is larger/smaller 
            // than multiplying it with the running product, often due to a 0)
            maxSoFar = Math.max(curr, maxSoFar * curr);
            minSoFar = Math.min(curr, minSoFar * curr);
            
            // Keep track of the absolute maximum found anywhere in the array
            result = Math.max(result, maxSoFar);
        }
        
        return result;
    }
}

/*
* COMPLEXITY:
* Time: O(N) (Single pass through the array).
* Space: O(1) (Only tracking scalar variables).
  */

````