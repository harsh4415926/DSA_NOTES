# Number of Islands (Online Queries) — GFG

## Problem Statement
- We have an `m x n` grid initialized with water (0).
- `operators` is a list of cells that will be converted into land (1) **sequentially**.
- After each addition of land, we need to return the **number of islands**.

---

## Approach: Disjoint Set Union (DSU)
- Treat each cell as a node in a DSU (index = `row * n + col`).
- Initially, all cells are water → no unions.
- When a cell becomes land:
    - Increase island count by 1.
    - For each of its **4 neighbors**:
        - If neighbor is land and in a different component, union them → island count decreases by 1.

**Why DSU?**
- Counting islands dynamically using BFS/DFS after each query is O(m*n*k) → too slow.
- DSU with **path compression + union by size/rank** gives near O(1) per query.

---

## DSU Implementation
```java
//copy disjoint set implementation 
class Solution {

    public List<Integer> numOfIslands(int m, int n, int[][] operators) {
        int[] dr = {-1, 0, 1, 0};
        int[] dc = {0, +1, 0, -1};

        int[][] vis = new int[m][n];
        DisjointSet ds = new DisjointSet(m * n);

        int count = 0;
        List<Integer> ans = new ArrayList<>();

        for (int i = 0; i < operators.length; i++) {
            int row = operators[i][0];
            int col = operators[i][1];

            // If already land, no change
            if (vis[row][col] == 1) {
                ans.add(count);
                continue;
            }

            // Make land → new island
            count++;
            vis[row][col] = 1;

            // Check 4 neighbors
            for (int j = 0; j < 4; j++) {
                int nrow = row + dr[j];
                int ncol = col + dc[j];

                if (nrow >= 0 && nrow < m && ncol >= 0 && ncol < n && vis[nrow][ncol] == 1) {
                    int node = row * n + col;
                    int newNode = nrow * n + ncol;

                    int ulp_node = ds.ulpar(node);
                    int ulp_newNode = ds.ulpar(newNode);

                    if (ulp_node != ulp_newNode) {
                        count--;
                        ds.unionBySize(node, newNode);
                    }
                }
            }

            ans.add(count);
        }

        return ans;
    }
}


