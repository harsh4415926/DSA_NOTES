# cycle detection in a directed graph using kahn's algorithm(topo sort)
## Problem
Detect if a directed graph contains a cycle using Kahn's algorithm (Topological Sort).
You are given V vertices and a list of directed edges. Return true if the graph has a cycle, otherwise false.

## Approach
1. Build the adjacency list from the edges.
2. Compute indegree for each vertex.
3. Push all vertices with indegree 0 into a queue.
4. Perform BFS (Kahn's algorithm):
    - Pop a node from the queue, increment the count of processed nodes.
    - Reduce the indegree of all its neighbors.
    - If any neighbor's indegree becomes 0, push it to the queue.
5. After processing, if the number of processed nodes != V, it means some nodes
   were never reached (cycle exists). Otherwise, the graph is acyclic.

## Code
class Solution {
public boolean isCyclic(int V, int[][] edges) {
List<List<Integer>> adj = new ArrayList<>();
for (int i = 0; i < V; i++) {
adj.add(new ArrayList<>());
}
for (int i = 0; i < edges.length; i++) {
adj.get(edges[i][0]).add(edges[i][1]);
}

        int[] indegree = new int[V];
        for (int i = 0; i < V; i++) {
            for (int j : adj.get(i)) {
                indegree[j]++;
            }
        }
        
        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0) {
                q.add(i);
            }
        }
        
        int count = 0;
        while (!q.isEmpty()) {
            int node = q.poll();
            count++;
            for (int nbr : adj.get(node)) {
                indegree[nbr]--;
                if (indegree[nbr] == 0) q.add(nbr);
            }
        }
        
        return count != V;  // true if cycle exists
    }
}

## Complexity
- Time: O(V + E) — building adjacency + processing each edge once.
- Space: O(V + E) — adjacency list + indegree array + queue.

