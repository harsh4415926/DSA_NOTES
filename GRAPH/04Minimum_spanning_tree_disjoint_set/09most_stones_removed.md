# LeetCode 947: Most Stones Removed with Same Row or Column

## Problem Statement
We are given `stones`, where each stone is at some coordinate `(row, col)` on a 2D plane.  
A stone can be removed if there is **another stone in the same row or column**.

We need to return the **maximum number of stones that can be removed**.

---

## Intuition
- If two stones share the same row or column, they are **connected**.
- Stones form **connected components** based on row/column links.
- In each connected component, we can remove all stones except **one representative**.
- So, the answer = **Total stones - Number of connected components**.

---

## Approach
1. Use **Disjoint Set Union (DSU / Union-Find)** to group stones:
    - Treat each **row** and each **column** as nodes in DSU.
    - For a stone `(row, col)`, union `row` with `col + offset`.
        - Offset ensures rows and columns don’t overlap in index space.
2. After processing all stones:
    - Count how many **unique connected components** exist that actually contain stones.
3. Answer = `stones.length - components`.

---

## Time Complexity
- Union-Find operations: **O(N * α(N))** ≈ **O(N)**, where `N = stones.length`.
- Counting components: **O(M + N)** where `M` = max row, `N` = max col.
- Overall: **O(N)**.

---

## Space Complexity
- DSU arrays for `rows + cols` → **O(M + N)**.

---

## Code (Java)

```java
class Solution {
    public int removeStones(int[][] stones) {
        int m = 0, n = 0;
        // Find max row and col to size DSU
        for (int i = 0; i < stones.length; i++) {
            int row = stones[i][0];
            int col = stones[i][1];
            m = Math.max(m, row);
            n = Math.max(n, col);
        }
        m++; n++;

        DisjointSet ds = new DisjointSet(m + n);

        // Union row with column (with offset)
        for (int[] it : stones) {
            int nodeRow = it[0];
            int nodeCol = it[1] + m + 1;
            ds.unionBySize(nodeRow, nodeCol);
        }

        // Count connected components that matter (size > 1)
        int compThatMatters = 0;
        for (int i = 0; i < m + n; i++) {
            if (ds.parent.get(i) == i && ds.size.get(i) > 1)
                compThatMatters++;
        }

        return stones.length - compThatMatters;
    }
}
