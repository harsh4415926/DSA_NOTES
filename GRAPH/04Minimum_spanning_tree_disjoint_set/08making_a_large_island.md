# LeetCode 827: Making A Large Island

## Problem Statement
We are given an `n x n` binary grid. Each cell contains either `0` (water) or `1` (land).  
We can change **at most one `0` to `1`**.  
The goal is to return the **size of the largest island** possible after this change.

- An island is a group of `1`s connected **4-directionally** (up, down, left, right).

---

## Intuition
- If we treat each connected component of `1`s as an island, we can merge them using **Disjoint Set Union (DSU / Union-Find)**.
- First, build DSU sets for existing islands.
- Then, for each `0`, check its **4-directional neighbors**:
    - Collect unique island parents.
    - Calculate total island size if we flip this `0` to `1`.
- Keep track of the maximum possible size.
- Finally, handle the case where the grid already consists of all `1`s (no `0` to flip).

---

## Approach
1. Use **Disjoint Set Union (DSU)** with **Union by Size** + **Path Compression**.
2. Traverse the grid:
    - For each `1`, union it with its adjacent `1`s.
3. Traverse the grid again:
    - For each `0`, check its **unique adjacent islands**.
    - Sum their sizes + `1` (for the flipped `0`).
    - Update the maximum island size.
4. Return the maximum found.

---

## Time Complexity
- Building DSU: **O(n² * α(n²))** (almost linear, α is inverse Ackermann).
- Checking each `0`: **O(4 * n²)** = **O(n²)**.
- Overall: **O(n²)**.

---

## Space Complexity
- DSU arrays (`parent`, `rank`, `size`) → **O(n²)**.
- Grid storage → **O(n²)**.
- Extra `HashSet` for neighbors → **O(4)** per cell.

---

## Code (Java)

```java
//disjoint set implementation
class DisjointSet {
    ArrayList<Integer> parent = new ArrayList<>();
    ArrayList<Integer> rank = new ArrayList<>();
    ArrayList<Integer> size = new ArrayList<>();

    DisjointSet(int n) {
        for (int i = 0; i <= n; i++) {
            parent.add(i);
            rank.add(0);
            size.add(1);
        }
    }

    public int ulpar(int n) {
        if (n == parent.get(n)) return n;
        int up = ulpar(parent.get(n));
        parent.set(n, up);
        return up;
    }

    public void unionBySize(int u, int v) {
        int ulp_u = ulpar(u);
        int ulp_v = ulpar(v);
        if (ulp_u == ulp_v) return;

        if (size.get(ulp_u) < size.get(ulp_v)) {
            parent.set(ulp_u, ulp_v);
            size.set(ulp_v, size.get(ulp_v) + size.get(ulp_u));
        } else {
            parent.set(ulp_v, ulp_u);
            size.set(ulp_u, size.get(ulp_u) + size.get(ulp_v));
        }
    }
}




class Solution {
    public int largestIsland(int[][] grid) {
        int n = grid.length;
        DisjointSet ds = new DisjointSet(n * n);
        int[] dr = {-1, 0, 1, 0};
        int[] dc = {0, 1, 0, -1};

        // Step 1: Build DSU for existing islands
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 0) continue;
                int node = i * n + j;
                for (int t = 0; t < 4; t++) {
                    int nrow = i + dr[t];
                    int ncol = j + dc[t];
                    if (nrow >= 0 && nrow < n && ncol >= 0 && ncol < n) {
                        if (grid[nrow][ncol] == 1) {
                            int newNode = nrow * n + ncol;
                            ds.unionBySize(node, newNode);
                        }
                    }
                }
            }
        }

        int max = Integer.MIN_VALUE;

        // Step 2: Try flipping each 0 to 1
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) continue;

                HashSet<Integer> set = new HashSet<>();
                for (int t = 0; t < 4; t++) {
                    int nrow = i + dr[t];
                    int ncol = j + dc[t];
                    if (nrow >= 0 && nrow < n && ncol >= 0 && ncol < n) {
                        if (grid[nrow][ncol] == 1) {
                            set.add(ds.ulpar(nrow * n + ncol));
                        }
                    }
                }

                int size = 1; // flip current 0
                for (int adjParent : set) {
                    size += ds.size.get(adjParent);
                }
                max = Math.max(max, size);
            }
        }

        // Step 3: Handle case where grid has no zero
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    int parent = ds.ulpar(i * n + j);
                    max = Math.max(max, ds.size.get(parent));
                }
            }
        }
        return max;
    }
}
```
## time complexity
- around O(n*n)
