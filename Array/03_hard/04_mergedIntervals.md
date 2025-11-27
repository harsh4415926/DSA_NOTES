# Problem: Merge Intervals
You are given an array of intervals, where intervals[i] = [start_i, end_i] represents a single interval. Your task is to merge all overlapping intervals and return a new array of the non-overlapping intervals that cover all the intervals in the input.

Example: Input: intervals = [[1, 3], [2, 6], [8, 10], [15, 18]] Output: [[1, 6], [8, 10], [15, 18]] Explanation: The intervals [1, 3] and [2, 6] overlap, so they are merged into [1, 6].

---
````java
class Solution {
    /**
     * Merges all overlapping intervals from a given array of intervals.
     * @param intervals An array of [start, end] pairs.
     * @return An array of merged, non-overlapping intervals.
     */
    public int[][] merge(int[][] intervals) {
        // Sort the intervals based on their starting points
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
        
        int n = intervals.length;
        List<int[]> merged = new ArrayList<>();
        
        // Add the first interval to our 'merged' list to start
        merged.add(intervals[0]);
        
        // Iterate through the rest of the intervals
        for (int i = 1; i < n; i++) {
            // Get the last interval added to our merged list
            int[] prev = merged.get(merged.size() - 1);
            int[] curr = intervals[i];
            
            // Check for overlap: if the current interval's start is before or at the previous interval's end
            if (curr[0] <= prev[1]) {
                // Merge: update the end of the 'prev' interval to be the maximum of both ends
                prev[1] = Math.max(prev[1], curr[1]);
            } else {
                // No overlap: add the current interval as a new, separate interval
                merged.add(curr);
            }
        }
        
        // Convert the List<int[]> back to a 2D array (int[][])
        int[][] ans = new int[merged.size()][2];
        for (int i = 0; i < ans.length; i++) {
            ans[i][0] = merged.get(i)[0];
            ans[i][1] = merged.get(i)[1];
        }

        return ans;

        // Time Complexity: O(N log N)
        // - Sorting the intervals takes O(N log N) time.
        // - The single pass to merge intervals takes O(N) time.
        // - The dominant factor is the sorting: O(N log N).

        // Space Complexity: O(N)
        // - In the worst case (no intervals merge), the 'merged' list will store
        //   all N intervals.
        // - The space for sorting can be O(log N) or O(N) depending on the algorithm.
    }
}
````