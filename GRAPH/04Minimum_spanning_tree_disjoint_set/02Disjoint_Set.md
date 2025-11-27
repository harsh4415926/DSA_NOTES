# **Disjoint Set (Union-Find)**

---

## **What problem does it solve?**
- Efficiently manages a collection of disjoint (non-overlapping) sets.
- Supports two main operations:
    1. **Find (ulpar)** → Determine which set an element belongs to.
    2. **Union** → Merge two sets into one.

Used in:
- **Cycle detection** in undirected graphs.
- **Minimum Spanning Tree (Kruskal’s Algorithm)**.
- **Connected components** problems.

---

## **Key Optimizations**
1. **Path Compression**
    - When finding the ultimate parent (`ulpar`), we directly link each node to its root.
    - This flattens the tree and speeds up future queries.

2. **Union by Rank or Size**
    - Always attach the smaller tree under the larger tree (by rank or size).
    - Keeps the resulting tree shallow, improving efficiency.

---

## **Code (Java)**

```java
class DisjointSet {
    ArrayList<Integer> parent = new ArrayList<>();
    ArrayList<Integer> rank = new ArrayList<>();
    ArrayList<Integer> size = new ArrayList<>();

    DisjointSet(int n) {
        // Initialize n separate sets: {1}, {2}, ..., {n}
        for (int i = 0; i <= n; i++) { // supports both 0-based and 1-based
            parent.add(i);     // each element is initially its own leader
            rank.add(0);       // used to keep trees balanced (if using rank)
            size.add(1);       // used to keep track of set sizes (if using size)
        }
    }

    public int ulpar(int n) {
        // Find ultimate leader (which set does n belong to?)
        if (n == parent.get(n)) return n;
        int up = ulpar(parent.get(n));
        parent.set(n, up); // Path compression: directly remember leader
        return up;
    }

    public void unionByRank(int u, int v) {
        // Merge sets containing u and v using rank to balance
        int ulp_u = ulpar(u);
        int ulp_v = ulpar(v);
        if (ulp_u == ulp_v) return; // already in same set

        if (rank.get(ulp_u) < rank.get(ulp_v)) {
            parent.set(ulp_u, ulp_v); // v's leader becomes overall leader
        } else if (rank.get(ulp_v) < rank.get(ulp_u)) {
            parent.set(ulp_v, ulp_u); // u's leader becomes overall leader
        } else {
            parent.set(ulp_u, ulp_v); // pick one leader arbitrarily
            rank.set(ulp_v, rank.get(ulp_v) + 1); // increase rank
        }
    }

    public void unionBySize(int u, int v) {
        // Merge sets containing u and v using size to balance
        int ulp_u = ulpar(u);
        int ulp_v = ulpar(v);
        if (ulp_u == ulp_v) return; // already same set

        if (size.get(ulp_u) < size.get(ulp_v)) {
            parent.set(ulp_u, ulp_v); // attach smaller set under bigger set
            size.set(ulp_v, size.get(ulp_v) + size.get(ulp_u));
        } else {
            parent.set(ulp_v, ulp_u);
            size.set(ulp_u, size.get(ulp_u) + size.get(ulp_v));
        }
    }
}

public class disjoint_set {
    public static void main(String[] args) {
        DisjointSet st = new DisjointSet(7);
        st.unionByRank(1, 2);
        st.unionByRank(2, 3);
        st.unionByRank(4, 5);
        st.unionBySize(6, 7);
        st.unionBySize(5, 6);

        if (st.ulpar(3) == st.ulpar(7))
            System.out.println("same component");
        else
            System.out.println("not same component");
    }
}
```
## time complexity
- O(4 α)=O(constant)=O(1);  // α is inverse Ackermann function
- 
