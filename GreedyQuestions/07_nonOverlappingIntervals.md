# non overlapping intervals
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 435 - Non-overlapping Intervals
 * ----------------------------------------------------------------------
 * LOGIC (Greedy Strategy):
 * 1. This is identical to the "Activity Selection / N Meetings" problem.
 * 2. Goal: find minimum number of intervals to delete to make intervals non-overlapping
 * 3. Sort by END time. Keeping intervals that end earliest leaves the 
 * most room for the rest.
 * 4. Answer: Minimum removals = (Total intervals) - (Max non-overlapping).
 * ----------------------------------------------------------------------
 */

class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        // Sort by end time ascending
        // Note: (a, b) -> Integer.compare(a[1], b[1]) is safer against overflow
        Arrays.sort(intervals, (a, b) -> a[1] - b[1]);
        
        int n = intervals.length;
        int count = 1; // At least one interval will be kept
        int lastEnd = intervals[0][1];
        
        for (int i = 1; i < n; i++) {
            int start = intervals[i][0];
            int end = intervals[i][1];
            
            // If current interval starts after or when the last one ends
            if (start >= lastEnd) {
                count++;       // Keep it
                lastEnd = end; // Update the boundary
            }
        }
        
        // Total intervals minus the maximum we can keep = minimum to erase
        return n - count;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N log N) (Sorting array).
 * Space: O(log N) (Built-in sorting stack space).
 */
````