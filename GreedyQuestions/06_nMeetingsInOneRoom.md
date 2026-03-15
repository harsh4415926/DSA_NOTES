# N meetings in one room(gfg)
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: Activity Selection / N Meetings in One Room
 * ----------------------------------------------------------------------
 * LOGIC (Greedy Strategy):
 * 1. Goal: Maximize count of non-overlapping meetings.
 * 2. Sort by END time (ascending).
 * - Why? Finishing early leaves the most room for subsequent meetings.
 * 3. Iterate: Select next meeting if it starts AFTER the last one ended.
 * ----------------------------------------------------------------------
 */

import java.util.*;

class Solution {
    
    static class Meeting {
        int start;
        int end;
        
        Meeting(int start, int end) {
            this.start = start;
            this.end = end;
        }
    }
    
    public static int maxMeetings(int start[], int end[]) {
        int n = start.length;
        Meeting[] meetings = new Meeting[n];
        
        for (int i = 0; i < n; i++) {
            meetings[i] = new Meeting(start[i], end[i]);
        }
        
        // Key Step: Sort by finish time.
        // If finish times are equal, sort order usually doesn't matter for count.
        Arrays.sort(meetings, (a, b) -> a.end - b.end);
        
        int count = 1;  // First meeting (earliest finish) is always optimal
        int lastEnd = meetings[0].end;
        
        for (int i = 1; i < n; i++) {
            // Check for overlap: Start time must be > previous End time
            if (meetings[i].start > lastEnd) {
                count++;
                lastEnd = meetings[i].end; // Update end time
            }
        }
        
        return count;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N log N) (Sorting dominates).
 * Space: O(N) (Meeting objects array).
 */
````
