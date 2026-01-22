# vertical order traversal 
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 987 - Vertical Order Traversal of a Binary Tree
 * ----------------------------------------------------------------------
 * Given the root of a binary tree, calculate the vertical order traversal.
 * * Coordinates (row, col) are assigned as follows:
 * - Root is at (0, 0).
 * - Left child: (row + 1, col - 1)
 * - Right child: (row + 1, col + 1)
 * * Rules for ordering in the result:
 * 1. Sort by Column index (left to right).
 * 2. If two nodes share the same column, sort by Row index (top to bottom).
 * 3. If two nodes share the same row AND column, sort by Value (ascending).
 * * * Example:
 * 3
 * / \
 * 9  20
 * /  \
 * 15   7
 * Output: [[9], [3, 15], [20], [7]]
 * ----------------------------------------------------------------------
 */


class Solution {

    // Helper method for recursive traversal (DFS - Preorder)
    // x represents the vertical column index (horizontal distance).
    // y represents the horizontal row index (depth).
    // Structure: Map<Column, Map<Row, PriorityQueue<Values>>>
    public void preorder(TreeNode root, TreeMap<Integer, TreeMap<Integer, PriorityQueue<Integer>>> xMap, int x, int y) {
        if (root == null) return;

        // Initialize the outer map for the current column 'x' if not present
        if (!xMap.containsKey(x)) {
            xMap.put(x, new TreeMap<>());
        }

        // Initialize the inner map for the current row 'y' within column 'x'
        if (!xMap.get(x).containsKey(y)) {
            // We use a PriorityQueue here to handle the "overlapping nodes" rule:
            // "If nodes share same row and col, sort by value."
            xMap.get(x).put(y, new PriorityQueue<>());
        }

        // Add the current node's value to the priority queue at coordinate (x, y)
        xMap.get(x).get(y).add(root.val);

        // Recursive calls:
        // Go Left: x decreases (moves left), y increases (depth increases)
        preorder(root.left, xMap, x - 1, y + 1);
        
        // Go Right: x increases (moves right), y increases (depth increases)
        preorder(root.right, xMap, x + 1, y + 1);
    }

    public List<List<Integer>> verticalTraversal(TreeNode root) {
        // We use TreeMap for 'xMap' to keep Columns sorted automatically (-2, -1, 0, 1, 2...)
        // Inner Map is also a TreeMap to keep Rows sorted automatically (0, 1, 2...)
        TreeMap<Integer, TreeMap<Integer, PriorityQueue<Integer>>> xMap = new TreeMap<>();
        
        // Populate the map using DFS
        // Note: Any traversal (inorder, postorder, level order) works as long as coordinates are passed correctly.
        preorder(root, xMap, 0, 0);

        List<List<Integer>> ans = new ArrayList<>();

        // Iterate over the columns (Outer Map Values)
        for (TreeMap<Integer, PriorityQueue<Integer>> yMap : xMap.values()) {
            List<Integer> list = new ArrayList<>();
            
            // Iterate over the rows (Inner Map Values)
            for (PriorityQueue<Integer> pq : yMap.values()) {
                // Extract elements from PriorityQueue to ensure they are sorted by value
                while (!pq.isEmpty()) {
                    list.add(pq.poll());
                }
            }
            ans.add(list);
        }
        return ans;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N log N)
 * - Traversal takes O(N).
 * - Insertions into TreeMap take O(log N) or O(log K) where K is width.
 * - Inserting into PriorityQueue takes O(log M) where M is nodes at specific (x,y).
 * - Fetching from maps and sorting (via PriorityQueue polling) dominates, generally O(N log N).
 * * * Space Complexity: O(N)
 * - We store every node in the map structure.
 * - Recursion stack space takes O(H) where H is tree height.
 * ----------------------------------------------------------------------
 */
````