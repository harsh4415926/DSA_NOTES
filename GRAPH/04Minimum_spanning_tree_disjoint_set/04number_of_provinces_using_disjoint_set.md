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
// dsu implementation
public int findCircleNum(int[][] isConnected) {
    int n = isConnected.length;
    DisjointSet ds = new DisjointSet(n);

    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            if (isConnected[i][j] == 1) {
                ds.unionBySize(i, j);
            }
        }
    }

    int provinces = 0;
    for (int i = 0; i < n; i++) {
        if (ds.ulPar(i) == i)
            provinces++;
    }

    return provinces;
}
}
```
## time complexity
- O(V²) for traversing adjacency matrix.
- Union-Find operations ≈ O(4 α) per union (almost constant).