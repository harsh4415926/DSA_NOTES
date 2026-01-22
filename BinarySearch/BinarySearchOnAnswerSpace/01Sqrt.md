# square root of a number
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 69 - Sqrt(x)
 * ----------------------------------------------------------------------
 * Given a non-negative integer x, return the square root of x rounded 
 * down to the nearest integer. The returned integer should be non-negative.
 * * You must not use any built-in exponent function or operator.
 * * Example 1: x = 4 -> Output: 2
 * Example 2: x = 8 -> Output: 2 (Explanation: The square root of 8 is 
 * 2.82842..., and since we round it down to the nearest integer, 2 is returned.)
 * ----------------------------------------------------------------------
 */

class Solution {
    public int mySqrt(int x) {
        // Edge Case: The square root of 0 is 0. 
        // Although the logic below handles it (ans initialized to 0), 
        // the search range usually starts from 1 for efficiency.
        if (x == 0) return 0;

        int low = 1;
        int high = x;
        int ans = 0;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            // IMPORTANT: Cast to (long) to prevent Integer Overflow.
            // If mid is close to Integer.MAX_VALUE, mid * mid will overflow int.
            long square = (long) mid * mid;

            if (square == x) {
                // Perfect square found
                return mid;
            } 
            else if (square < x) {
                // If mid*mid < x, then mid is a candidate for the answer.
                // However, there might be a larger number closer to the actual root.
                // We store mid in 'ans' and try searching the right half.
                ans = mid;
                low = mid + 1;
            } 
            else {
                // If mid*mid > x, mid is too large.
                // We must search the left half.
                high = mid - 1;
            }
        }
        
        // Return the best candidate found. 
        // Alternatively, 'return high' also works because when the loop ends, 
        // high is the largest integer such that high*high <= x.
        return ans;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(log x)
 * - We are performing Binary Search on the range [1, x].
 * * Space Complexity: O(1)
 * - We use constant extra space for variables.
 * ----------------------------------------------------------------------
 */
````