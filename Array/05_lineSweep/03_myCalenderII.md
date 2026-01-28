# my calender II
## using line sweep
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 731 - My Calendar II
 * ----------------------------------------------------------------------
 * Implement a calendar that allows a "double booking" (overlap of 2 events),
 * but returns false if a "triple booking" (overlap of 3) would occur.
 * * STEPS:
 * 1. Add new event to TreeMap (+1 at start, -1 at end).
 * 2. Sweep line to check overlaps. If overlap > 2 -> Revert changes & return false.
 * ----------------------------------------------------------------------
 */

class MyCalendarTwo {
    // Map stores the difference array on the timeline. Key = Time, Value = Change.
    TreeMap<Integer, Integer> map;
   
    public MyCalendarTwo() {
        map = new TreeMap<>();
    }
    
    public boolean book(int startTime, int endTime) {
        // 1. Optimistically add the booking
        map.put(startTime, map.getOrDefault(startTime, 0) + 1);
        map.put(endTime, map.getOrDefault(endTime, 0) - 1);
        
        int currSum = 0;
        
        // 2. Line Sweep to validate
        // Iterate through all time points in order
        for (int val : map.values()) {
            currSum += val;
            
            // If active overlaps exceed 2, this is a triple booking (Invalid)
            if (currSum > 2) {
                // Revert the changes made at the start of the function
                map.put(startTime, map.get(startTime) - 1);
                map.put(endTime, map.get(endTime) + 1);
                
                // Cleanup: Remove keys if their count becomes 0 to save space/time
                if (map.get(startTime) == 0) map.remove(startTime);
                if (map.get(endTime) == 0) map.remove(endTime);
                
                return false;
            }
        }
        
        // If loop completes without currSum > 2, the booking is valid
        return true;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N^2)
 * - In the worst case, for every new booking (N total), we traverse the 
 * entire TreeMap which has up to 2*N entries.
 * * Space Complexity: O(N)
 * - Stores start and end points for all valid bookings.
 * ----------------------------------------------------------------------
 */
````
