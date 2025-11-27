# Problem: M-Coloring Problem (Graph Coloring)
You are given an undirected graph with v vertices (numbered 0 to v-1) and a list of edges. You are also given an integer m, which represents the number of available colors.

Your task is to determine if it's possible to color all the vertices of the graph using at most m colors, such that no two adjacent vertices (i.e., vertices connected by an edge) have the same color. Return true if such a coloring is possible, and false otherwise. This is a classic backtracking problem.

Example: Input: v = 4, m = 3, edges = [[0, 1], [1, 2], [2, 3], [3, 0], [0, 2]] Output: true Explanation: A valid coloring is {Node 0: Color 1, Node 1: Color 2, Node 2: Color 3, Node 3: Color 2}.

---
## using backtracking
````java
class Solution {
    
    /**
     * Checks if it's safe to assign a specific color 'col' to 'node'.
     * It's safe if none of the node's neighbors already have that color.
     * @param node The vertex to check.
     * @param col The color to assign.
     * @param adj The adjacency list of the graph.
     * @param color The array tracking current color assignments (0 = uncolored).
     * @return true if safe to color, false otherwise.
     */
    boolean isSafe(int node, int col, List<List<Integer>> adj, int[] color) {
        // Iterate over all neighbors of the current node
        for (int nbr : adj.get(node)) {
            // If a neighbor already has the same color, it's not safe
            if (color[nbr] == col) return false; // No need to check color[nbr] != 0, as col >= 1
        }
        return true;
    }

    /**
     * Recursive backtracking function to try and color the graph.
     * @param node The current node we are trying to color.
     * @param adj The adjacency list.
     * @param color The color assignment array.
     * @param m The total number of available colors.
     * @return true if a valid coloring is found, false otherwise.
     */
    boolean colorNode(int node, List<List<Integer>> adj, int[] color, int m) {
        // Base case: If we have successfully colored all nodes (0 to v-1)
        if (node == adj.size()) return true;

        // Try every possible color (from 1 to m) for the current node
        for (int col = 1; col <= m; col++) {
            // Check if this color is valid for this node
            if (isSafe(node, col, adj, color)) {
                color[node] = col; // Assign the color

                // Recurse for the next node
                if (colorNode(node + 1, adj, color, m)) {
                    return true; // If the rest of the graph can be colored, we are done
                }
                
                // Backtrack: If the recursive call failed, reset the color
                color[node] = 0; 
            }
        }
        // If no color (from 1 to m) worked for this node, return false
        return false;
    }

    /**
     * Entry point for the M-Coloring problem.
     * @param v The number of vertices.
     * @param edges The list of edges in the graph.
     * @param m The number of colors available.
     * @return true if the graph can be m-colored, false otherwise.
     */
    boolean graphColoring(int v, int[][] edges, int m) {
        // Build the adjacency list representation of the graph
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < v; i++) adj.add(new ArrayList<>());
        
        for (int i = 0; i < edges.length; i++) {
            adj.get(edges[i][0]).add(edges[i][1]);
            adj.get(edges[i][1]).add(edges[i][0]); // Undirected graph
        }
        
        // 'color' array stores the color assigned to each vertex (0 = uncolored)
        int[] color = new int[v]; 

        // Start the backtracking process from the first node (node 0)
        return colorNode(0, adj, color, m);

        // Time Complexity: O(m^V)
        // - In the worst case, the recursive function 'colorNode' tries 'm' colors
        //   for each of the 'V' vertices.
        // - The 'isSafe' check inside the loop takes O(V) (or O(degree)) time.
        // - This results in a total time complexity of O(V * m^V), but it's
        //   commonly expressed as O(m^V) as it's the dominant factor.

        // Space Complexity: O(V + E)
        // - O(V + E) for storing the adjacency list 'adj'.
        // - O(V) for the 'color' array.
        // - O(V) for the recursion stack depth in the worst case.
        // - Total space is dominated by the adjacency list and recursion stack.
    }
}
````