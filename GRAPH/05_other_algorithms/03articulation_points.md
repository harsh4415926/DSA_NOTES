# Problem: Articulation Points in a Graph (GFG)

## Problem Description
Find all **articulation points** in an undirected graph.  
An **articulation point** is a vertex that, when removed (with its edges), increases the number of connected components of the graph.

**Input:**
- `V` = number of vertices
- `adj` = adjacency list

**Output:**
- A list of articulation points in ascending order
- If there are none, return `[-1]`

---

## Intuition & Approach
This is solved using **Tarjan's Algorithm** (DFS with timestamps):
- `tin[u]`: discovery time of node `u` during DFS.
- `low[u]`: lowest discovery time reachable from `u` (including back edges).

### Key observations:
1. For root of DFS tree (parent = -1):
    - If it has more than 1 child, it's an articulation point.
2. For other nodes:
    - If any child `v` of `u` satisfies `low[v] >= tin[u]`, then `u` is an articulation point.(unlike the bridges problem here it is wrong that low[v]=tin[u]then u is not an articulatio point because it is not enough to reach u v must reach a node lesser than u then only its not an articulation point)
3. unlike bridges qn here if nbr is visited we say low[node]=min(low[node],tin[nbr]) 
### Steps:
1. Run DFS from every unvisited node (handle disconnected graph).
2. Maintain `tin[]`, `low[]`, `mark[]` to track articulation points.
3. After DFS, collect all nodes where `mark[node] == 1`.
4. If none found, return `[-1]`.

---

## Code
```java
class Solution {
    int timer = 0;

    public void dfs(int node, int parent, int[] vis, int[] tin, int[] low,
                    int[] mark, ArrayList<ArrayList<Integer>> adj) {
        vis[node] = 1;
        tin[node] = low[node] = timer++;
        int childs = 0;

        for (int nbr : adj.get(node)) {
            if (vis[nbr] == 0) {
                childs++;
                dfs(nbr, node, vis, tin, low, mark, adj);
                low[node] = Math.min(low[node], low[nbr]);

                // Non-root articulation point condition
                if (low[nbr] >= tin[node] && parent != -1) 
                    mark[node] = 1;
            } 
            else if (nbr != parent) {
                // Back edge
                low[node] = Math.min(low[node], tin[nbr]);
            }
        }

        // Root articulation point condition
        if (childs > 1 && parent == -1) 
            mark[node] = 1;
    }

    public ArrayList<Integer> articulationPoints(int V, ArrayList<ArrayList<Integer>> adj) {
        int[] vis = new int[V];
        int[] mark = new int[V];
        int[] tin = new int[V];
        int[] low = new int[V];

        for (int i = 0; i < V; i++) {
            if (vis[i] == 0) {
                dfs(i, -1, vis, tin, low, mark, adj);
            }
        }

        ArrayList<Integer> ans = new ArrayList<>();
        for (int i = 0; i < V; i++) {
            if (mark[i] == 1) ans.add(i);
        }

        if (ans.size() == 0) ans.add(-1);
        return ans;
    }
}
```
Time Complexity  
DFS traversal: O(V + E)  
Overall: O(V + E)

Space Complexity  
Arrays vis, tin, low, mark: O(V)  
Recursion stack: O(V)  
Overall: O(V)bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb