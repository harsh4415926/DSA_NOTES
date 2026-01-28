# meeting rooms II (gfg)
## using line sweep
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: Meeting Rooms II (Minimum Platforms/Meeting Rooms)
 * ----------------------------------------------------------------------
 * Given start and end times of meetings, find the minimum number of rooms required.
 * * STEPS:
 * 1. Use TreeMap to store events: Key = Time, Value = Net Change (+1 start, -1 end).
 * 2. Sweep through values (sorted by time) -> Accumulate count -> Track max overlap.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int minMeetingRooms(int[] start, int[] end) {
        // TreeMap keeps keys (time points) sorted automatically.
        // Map: Time -> Change in number of meetings
        TreeMap<Integer, Integer> events = new TreeMap<>();
        
        // 1. Build the Timeline
        for (int i = 0; i < start.length; i++) {
            // Meeting starts: increment room count
            events.put(start[i], events.getOrDefault(start[i], 0) + 1);
            
            // Meeting ends: decrement room count
            // Note: If start time equals end time of another, they cancel out in the map value,
            // effectively handling the [start, end) or non-overlapping condition correctly.
            events.put(end[i], events.getOrDefault(end[i], 0) - 1);
        }
        
        int currMeetings = 0;
        int maxMeetings = 0; // Initialize to 0 (handle empty case)
        
        // 2. Line Sweep
        // Iterate through the changes in chronological order
        for (int val : events.values()) {
            currMeetings += val;
            
            // The peak value of currMeetings represents the max concurrent meetings
            maxMeetings = Math.max(currMeetings, maxMeetings);
        }
        
        return maxMeetings;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N log N)
 * - We perform 2*N insertions into the TreeMap. Each insertion takes O(log N).
 * - Iterating values takes O(N).
 * * Space Complexity: O(N)
 * - TreeMap stores up to 2*N distinct time points.
 * ----------------------------------------------------------------------
 */
````