# find eventual safe states(leetcode 802)
## Problem
Find all eventual safe nodes in a directed graph.
A node is safe if every possible path starting from it leads to a terminal node (no outgoing edges).
Return a list of all safe nodes sorted in ascending order.

## Approach
0. in reversed graph,using kahn's algo remove node with indegree 0 iteratively ; all nodes removed this way are safe
1. Reverse the graph: for each edge u → v, add v → u in the reversed graph.
2. Compute indegree for each node in the reversed graph.
3. Apply Kahn's algorithm (topological sort):
    - Add all nodes with indegree 0 to a queue.
    - While the queue is not empty:
        - Pop a node, add it to the safe nodes list.
        - Decrease indegree of all its neighbors in the reversed graph.
        - If any neighbor's indegree becomes 0, add it to the queue.
4. Sort the list of safe nodes before returning.

## Code
class Solution {
public List<Integer> eventualSafeNodes(int[][] graph) {
List<List<Integer>> revAdj = new ArrayList<>();
int V = graph.length;
for (int i = 0; i < V; i++) {
revAdj.add(new ArrayList<>());
}
for (int i = 0; i < V; i++) {
for (int it : graph[i]) {
revAdj.get(it).add(i);
}
}

        int[] indegree = new int[V];
        for (int i = 0; i < V; i++) {
            for (int it : revAdj.get(i)) {
                indegree[it]++;
            }
        }

        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0) q.add(i);
        }

        List<Integer> safeStates = new ArrayList<>();
        while (!q.isEmpty()) {
            int node = q.poll();
            safeStates.add(node);
            for (int nbr : revAdj.get(node)) {
                indegree[nbr]--;
                if (indegree[nbr] == 0) q.add(nbr);
            }
        }

        Collections.sort(safeStates);
        return safeStates;
    }
}

## Complexity
- Time: O(V + E) — building reversed adjacency list + BFS traversal.
- Space: O(V + E) — reversed adjacency list + indegree array + queue + safe nodes list.
  */
