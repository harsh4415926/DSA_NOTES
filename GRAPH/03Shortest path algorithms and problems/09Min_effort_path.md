# leetcode 1631: Path With Minimum Effort

## Problem Description
You are given an `m x n` grid `heights`, where `heights[r][c]` represents the height of a cell.  
You can move up, down, left, or right to an adjacent cell. The **effort** of a path is the maximum absolute difference in heights between two consecutive cells on that path.  
Return the minimum effort required to travel from `(0,0)` to `(m-1,n-1)`.

**Example:**  
Input: `[[1,2,2],[3,8,2],[5,3,5]]`  
Output: `2`

## Intuition & Approach
- This is similar to **Dijkstra's algorithm** for weighted graphs:
    - Treat each cell as a node.
    - Edge cost = absolute height difference between cells.
- Maintain a 2D array `effort[r][c]` to track the minimum effort to reach each cell.
- Use a min-heap priority queue `(effort, row, col)` to process nodes in increasing order of current effort.
- For each neighbor, update `effort[nr][nc] = max(currentEffort, diff)` if it's smaller than the previous stored effort.
- Once we reach `(m-1,n-1)`, we return the effort.

## Code
```java
class Tuple {
    int first, second, third;
    Tuple(int first, int second, int third) {
        this.first = first;
        this.second = second;
        this.third = third;
    }
}

class Solution {
    public int minimumEffortPath(int[][] heights) {
        int m = heights.length, n = heights[0].length;
        int[] dr = {-1, 0, 1, 0};
        int[] dc = {0, 1, 0, -1};
        
        int[][] effort = new int[m][n];
        for (int i = 0; i < m; i++) {
            Arrays.fill(effort[i], Integer.MAX_VALUE);
        }
        effort[0][0] = 0;

        PriorityQueue<Tuple> q = new PriorityQueue<>((a,b) -> a.third - b.third);
        q.add(new Tuple(0, 0, 0));

        while (!q.isEmpty()) {
            Tuple t = q.poll();
            int row = t.first, col = t.second, eff = t.third;

            if (row == m-1 && col == n-1) return eff;

            for (int i = 0; i < 4; i++) {
                int nrow = row + dr[i];
                int ncol = col + dc[i];
                if (nrow >= 0 && nrow < m && ncol >= 0 && ncol < n) {
                    int newEffort = Math.max(eff, Math.abs(heights[nrow][ncol] - heights[row][col]));
                    if (newEffort < effort[nrow][ncol]) {
                        effort[nrow][ncol] = newEffort;
                        q.add(new Tuple(nrow, ncol, newEffort));
                    }
                }
            }
        }
        return -1; // unreachable case
    }
}
```

## Final Complexities
- **Time Complexity:** `O(E * logV)`  (dijsktra)
                      = O(m * n * 4 * log(m*n))
- **Space Complexity:** O(m*n)

 