# LeetCode 1319 – Make Network Connected using Disjoint Set (Union-Find)

---

## Problem Idea

- We are given `n` computers (nodes) and a list of `connections`.
- Each connection `[u, v]` represents a cable connecting computer `u` and `v`.
- Goal: Connect all computers with **minimum number of operations**, where an operation is moving an existing cable.
- **Key Insight:**
    - If there are enough extra cables (`freeEdges`), we can reconnect components.
    - The minimum operations needed = **number of disconnected components - 1**.

---

## Thought Process

1. **Initialize Disjoint Set** for `n` nodes.
    - Each node initially belongs to its own component.

2. **Traverse connections**:
    - For each connection `(u, v)`:
        - If `u` and `v` are already in the same component → increment `freeEdges` (extra cable).
        - Else → union `u` and `v`.

3. **Count components**:
    - Iterate all nodes, count how many are **ultimate parents** → `component`.

4. **Compute operations needed**:
    - `connectionsRequired = component - 1`
    - If `freeEdges < connectionsRequired` → return `-1` (not enough cables).
    - Else → return `connectionsRequired`.

---

## Java code

```java
//dijoint set implementation
class DisjointSet {
    ArrayList<Integer> parent = new ArrayList<>();
    ArrayList<Integer> rank = new ArrayList<>();
    ArrayList<Integer> size = new ArrayList<>();

    DisjointSet(int n) {
        for (int i = 0; i <= n; i++) {
            parent.add(i);  // initially each node is its own leader
            rank.add(0);    // for union by rank
            size.add(1);    // for union by size
        }
    }

    public int ulpar(int n) {
        if (n == parent.get(n)) return n;
        int up = ulpar(parent.get(n));
        parent.set(n, up); // path compression
        return up;
    }

    public void unionByRank(int u, int v) {
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

//actual problem
class Solution {
    public int makeConnected(int n, int[][] connections) {
        DisjointSet ds = new DisjointSet(n);
        int freeEdges = 0;

        // Step 1: Union nodes if not already connected, else count free edges
        for (int i = 0; i < connections.length; i++) {
            int u = connections[i][0];
            int v = connections[i][1];

            if (ds.ulpar(u) == ds.ulpar(v)) {
                freeEdges++; // extra cable
                continue;
            }
            ds.unionBySize(u, v);
        }

        // Step 2: Count number of components
        int component = 0;
        for (int i = 0; i < n; i++) {
            if (ds.parent.get(i) == i) component++;
        }

        // Step 3: Compute operations required
        int connectionsRequired = component - 1;
        if (freeEdges < connectionsRequired) return -1;
        else return connectionsRequired;
    }
}
```
!
