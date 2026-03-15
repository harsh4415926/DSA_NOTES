# all nodes distance k in binary tree
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 863 - All Nodes Distance K in Binary Tree
 * ----------------------------------------------------------------------
 * LOGIC (Tree -> Graph Conversion):
 * 1. Trees are directed (Parent -> Child). To move up, we need undirected edges.
 * 2. buildGraph: Convert Tree to Adjacency List (Map<Node, Neighbors>).
 * 3. DFS/BFS: Start from 'target', treat as graph, stop at distance K.
 * ----------------------------------------------------------------------
 */

class Solution {
    // Map acts as Adjacency List for the graph
    Map<TreeNode, List<TreeNode>> graph = new HashMap<>();
    
    // STEP 1: Build Undirected Graph (add parent pointers)
    public void buildGraph(TreeNode root, TreeNode parent) {
        if (root == null) return;
        
        graph.putIfAbsent(root, new ArrayList<>());
        
        if (parent != null) {
            graph.get(root).add(parent);  // Edge Node -> Parent
            graph.get(parent).add(root);  // Edge Parent -> Node
        }
        
        buildGraph(root.left, root);
        buildGraph(root.right, root);
    }
    
    // STEP 2: DFS to find nodes at distance K
    public void dfs(TreeNode node, TreeNode parent, int level, int k, List<Integer> ans) {
        if (node == null) return;
        
        // Base case: Distance K reached
        if (level == k) {
            ans.add(node.val);
            return; 
        }
        
        // Traverse all neighbors (Left, Right, and Parent)
        for (TreeNode nbr : graph.get(node)) {
            // Avoid going back to where we came from (visited check)
            if (nbr != parent) {
                dfs(nbr, node, level + 1, k, ans);
            }
        }
    }
    
    public List<Integer> distanceK(TreeNode root, TreeNode target, int k) {
        buildGraph(root, null);
        
        List<Integer> ans = new ArrayList<>();
        // Start DFS from the TARGET node, not root
        dfs(target, null, 0, k, ans); 
        return ans;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N) (Build graph + DFS).
 * Space: O(N) (Adjacency Map + Recursion stack).
 */
````