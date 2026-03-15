# insert Interval
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 57 - Insert Interval
 * ----------------------------------------------------------------------
 * LOGIC (3-Phase Sweep):
 * 1. LEFT: Add all intervals completely BEFORE the new interval.
 * 2. MERGE: Overlap exists if current start <= new end. Merge them 
 * by expanding the new interval's start/end boundaries.
 * 3. RIGHT: Add all remaining intervals completely AFTER the new interval.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        int n = intervals.length;
        List<int[]> list = new ArrayList<>();
        int i = 0;
        
        // Phase 1: Skip & add intervals ending before the new interval starts
        while (i < n && intervals[i][1] < newInterval[0]) {
            list.add(intervals[i]);
            i++;
        }
        
        // Phase 2: Merge overlapping intervals
        // Condition: Current interval starts before or when new interval ends
        while (i < n && intervals[i][0] <= newInterval[1]) {
            // Expand the boundary of newInterval to encompass overlapping intervals
            newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
            newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
            i++;
        }
        // Add the fully merged interval
        list.add(newInterval);
        
        // Phase 3: Add remaining intervals
        while (i < n) {
           list.add(intervals[i]);
           i++;
        }
       
        // Convert List to 2D Array
        int[][] ans = new int[list.size()][2];
        for (i = 0; i < ans.length; i++) {
            ans[i] = list.get(i);
        }
        return ans;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N) (Single pass through the intervals).
 * Space: O(N) (For the result list).
 */
````