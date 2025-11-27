## DFS of Graph

**Intuition:**
- Perform a depth-first traversal of all nodes using recursion.
- Use a visited array to avoid revisiting nodes.
- Start DFS from every unvisited node to ensure disconnected components are also covered.
- Add each node to the result when first visited, then recursively visit its neighbors.

**Code:**
```java
class Solution {
    // Function to return a list containing the DFS traversal of the graph.
    public ArrayList<Integer> dfs(ArrayList<ArrayList<Integer>> adj) {
        int n = adj.size();
        ArrayList<Integer> ans = new ArrayList<>();
        int[] vis = new int[n];
        
        for(int i = 0; i < n; i++) {
            if(vis[i] == 0)
                dfs(i, adj, vis, ans);
        }
        return ans;
    }
    
    public void dfs(int node, ArrayList<ArrayList<Integer>> adj, int[] vis, ArrayList<Integer> ans) {
        vis[node] = 1;
        ans.add(node);
        for(int nbr : adj.get(node)) {
            if(vis[nbr] == 0) {
                dfs(nbr, adj, vis, ans);
            }
        }
    }
}
```
## time complexity-
- O(V + 2E) - O(V+E)      where 2E is total degree and V is no. of nodes
## space complexity
- O(V)      
