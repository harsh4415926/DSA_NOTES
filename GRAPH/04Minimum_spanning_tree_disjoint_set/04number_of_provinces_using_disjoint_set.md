# Number of Provinces – Using Disjoint Set (Union-Find)

---

## Problem Idea

- We are given a graph represented as an adjacency matrix `adj[V][V]`.
- **Province** = a group of directly or indirectly connected cities.
- Goal: Count the number of provinces (connected components) using **Disjoint Set**.

---

## Thought Process

1. **Initialize Disjoint Set** for all `V` nodes (cities).
    - Each city initially belongs to its own component.

2. **Traverse adjacency matrix**:
    - For every pair `(i, j)`:
        - If `adj[i][j] == 1` and `i != j`, **union** them.  
          (They belong to the same province.)

3. **Count ultimate parents**:
    - After all unions, the number of ultimate parents gives the number of provinces.
    - Reason: Each ultimate parent represents a distinct component.

---

## Disjoint Set Implementation

```java
//disjointSetimplementation
class DisjointSet {
    ArrayList<Integer> parent = new ArrayList<>();
    ArrayList<Integer> rank = new ArrayList<>();
    ArrayList<Integer> size = new ArrayList<>();

    DisjointSet(int n) {
        // Initialize n separate sets
        for (int i = 0; i <= n; i++) {
            parent.add(i);     // initially each node is its own leader
            rank.add(0);       // used for union by rank
            size.add(1);       // used for union by size
        }
    }

    public int ulpar(int n) {
        // Find ultimate parent with path compression
        if (n == parent.get(n)) return n;
        int up = ulpar(parent.get(n));
        parent.set(n, up); // compress path
        return up;
    }

    public void unionByRank(int u, int v) {
        // Merge sets using rank
        int ulp_u = ulpar(u);
        int ulp_v = ulpar(v);
        if (ulp_u == ulp_v) return;

        if (rank.get(ulp_u) < rank.get(ulp_v)) {
            parent.set(ulp_u, ulp_v);
        } else if (rank.get(ulp_v) < rank.get(ulp_u)) {
            parent.set(ulp_v, ulp_u);
        } else {
            parent.set(ulp_u, ulp_v);
            rank.set(ulp_v, rank.get(ulp_v) + 1);
        }
    }

    public void unionBySize(int u, int v) {
        // Merge sets using size
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
    static int numProvinces(ArrayList<ArrayList<Integer>> adj, int V) {
        // Step 1: Initialize Disjoint Set
        DisjointSet ds = new DisjointSet(V);

        // Step 2: Union nodes if connected
        for (int i = 0; i < V; i++) {
            for (int j = 0; j < V; j++) {
                if (adj.get(i).get(j) == 1 && i != j) {
                    ds.unionBySize(i, j);
                }
            }
        }

        // Step 3: Count ultimate parents → number of provinces
        int count = 0;
        for (int i = 0; i < V; i++) {
            if (ds.parent.get(i) == i) count++;
        }

        return count;
    }
}
```
## time complexity
- O(V²) for traversing adjacency matrix.
- Union-Find operations ≈ O(4 α) per union (almost constant).