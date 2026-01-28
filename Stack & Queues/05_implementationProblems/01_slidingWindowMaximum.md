# sliding window maximum
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 239 - Sliding Window Maximum
 * ----------------------------------------------------------------------
 * Return the max value in every sliding window of size k moving from left to right.
 * * STEPS:
 * 1. Maintain a Deque of indices with values in decreasing order (Front = Max).
 * 2. Remove out-of-bound indices -> Remove smaller elements from back -> Add current -> Record Max.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        int[] ans = new int[n - k + 1];
        int idx = 0;
        
        // Deque stores INDICES. 
        // Logic: Keep elements in decreasing order of their values.
        Deque<Integer> dq = new ArrayDeque<>();

        for (int i = 0; i < n; i++) {
            // 1. Remove indices that are out of the current window [i-k+1 ... i]
            // If the front index is too old (less than start of window), remove it.
            while (!dq.isEmpty() && dq.peekFirst() < (i - k + 1)) {
                dq.removeFirst();
            }

            // 2. Maintain Decreasing Order (Monotonic Queue)
            // If current element is larger than last elements in deque, those last elements
            // can NEVER be the maximum again (since current is larger and newer). Remove them.
            while (!dq.isEmpty() && nums[dq.peekLast()] <= nums[i]) {
                dq.removeLast();
            }

            // 3. Add current index to the deque
            dq.addLast(i);

            // 4. Record result
            // Once we have processed at least k elements (i >= k-1), the front is the max.
            if (i >= k - 1) {
                ans[idx++] = nums[dq.peekFirst()];
            }
        }
        return ans;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N)
 * - Each element is added to the Deque once and removed at most once.
 * * Space Complexity: O(k)
 * - The Deque stores at most k elements (indices) at any time.
 * ----------------------------------------------------------------------
 */
````