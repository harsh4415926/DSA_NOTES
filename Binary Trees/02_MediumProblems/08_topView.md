# top view
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: Top View of Binary Tree
 * ----------------------------------------------------------------------
 * Print nodes visible from the top, sorted by horizontal distance (column).
 * * LOGIC:
 * 1. Use TreeMap to store (Column -> {Row, Value}). Keeps columns sorted.
 * 2. DFS Traversal: maintain current (row, col).
 * 3. Update Map: If col is empty OR current node is higher (row < existing_row).
 * ----------------------------------------------------------------------
 */

class Solution {
    static class Pair {
        TreeNode node;
        int col;
        Pair(TreeNode node, int col) {
            this.node = node;
            this.col = col;
        }
    }

    public List<Integer> topView(TreeNode root) {
        TreeMap<Integer, Integer> map = new TreeMap<>();
        Queue<Pair> q = new LinkedList<>();

        q.offer(new Pair(root, 0));

        while (!q.isEmpty()) {
            Pair p = q.poll();
            TreeNode node = p.node;
            int col = p.col;

            if (!map.containsKey(col)) {
                map.put(col, node.val);
            }

            if (node.left != null)
                q.offer(new Pair(node.left, col - 1));
            if (node.right != null)
                q.offer(new Pair(node.right, col + 1));
        }

        return new ArrayList<>(map.values());
    }
}


/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N log N)
 * - N nodes traversal. TreeMap insertion takes O(log N).
 * - (Optimization: HashMap + Min/Max variable tracking reduces to O(N) but requires sorting keys later).
 * * Space Complexity: O(N)
 * - Map and Recursion Stack.
 * ----------------------------------------------------------------------
 */
````
