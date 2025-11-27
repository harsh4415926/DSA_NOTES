## Topological Sort (Kahn's Algorithm - BFS-based)

**Problem Description:**  
Given a directed graph with `V` vertices and `E` edges, return a topological ordering of the graph.  
A *topological order* is a linear ordering of vertices such that for every directed edge `u → v`, vertex `u` comes before `v` in the ordering.  
*(This is only possible for Directed Acyclic Graphs – DAGs.)*

**Intuition:**
- Count indegree (number of incoming edges) for every vertex.
- Start with all nodes having `indegree = 0` ,add them to queue.
- Repeatedly remove nodes with `indegree = 0` from the queue,add them to ans and reduce indegree of their neighbors.if indegree of neightbour becomes 0 add them to queue
- If at the end we processed all nodes, we have a valid topological order.

**Code:**
```java
class Solution {
    public static ArrayList<Integer> topoSort(int V, int[][] edges) {
        // Step 1: Build adjacency list
        List<List<Integer>> adj = new ArrayList<>();
        for(int i = 0; i < V; i++) {
            adj.add(new ArrayList<>());
        }
        for(int i = 0; i < edges.length; i++) {
            adj.get(edges[i][0]).add(edges[i][1]);
        }

        // Step 2: Compute indegree of all nodes
        int[] indegree = new int[V];
        for(int i = 0; i < V; i++) {
            for(int nbr : adj.get(i)) {
                indegree[nbr]++;
            }
        }

        // Step 3: Push all nodes with indegree 0 into queue
        Queue<Integer> q = new LinkedList<>();
        for(int i = 0; i < V; i++) {
            if(indegree[i] == 0) q.add(i);
        }

        // Step 4: Perform BFS
        ArrayList<Integer> ans = new ArrayList<>();
        while(!q.isEmpty()) {
            int node = q.poll();
            ans.add(node);

            // Reduce indegree of neighbors
            for(int nbr : adj.get(node)) {
                indegree[nbr]--;
                if(indegree[nbr] == 0) q.add(nbr);
            }
        }
        return ans;//since the graph is surely DAG then it is confirm that all the nodes are processed so no need to check for it 
    }
}
```
Time Complexity: O(V + E)  
Each vertex and edge is processed once.

Space Complexity: O(V)  
For adjacency list, indegree array, and queue.
