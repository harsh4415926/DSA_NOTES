# Problem: Merge Intervals
You are given an array of intervals, where intervals[i] = [start_i, end_i] represents a single interval. Your task is to merge all overlapping intervals and return a new array of the non-overlapping intervals that cover all the intervals in the input.

Example: Input: intervals = [[1, 3], [2, 6], [8, 10], [15, 18]] Output: [[1, 6], [8, 10], [15, 18]] Explanation: The intervals [1, 3] and [2, 6] overlap, so they are merged into [1, 6].

---
````java
class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);

        List<int[]> res = new ArrayList<>();

        for (int[] interval : intervals) {
            // if no overlap → add new interval
            if (res.isEmpty() || res.get(res.size() - 1)[1] < interval[0]) {
                res.add(interval);
            }
            // overlap → merge
            else {
                res.get(res.size() - 1)[1] = Math.max(
                        res.get(res.size() - 1)[1],
                        interval[1]
                );
            }
        }

        return res.toArray(new int[res.size()][]);
    }
}
````