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
public int makeConnected(int n, int[][] connections) {

    // At least (n - 1) cables are required to connect n nodes
    if (connections.length < n - 1)
        return -1;
    
    int provinces = findProvince(n, connections);

    // To connect 'provinces' components, we need (provinces - 1) operations
    return provinces - 1;
}

```
!
