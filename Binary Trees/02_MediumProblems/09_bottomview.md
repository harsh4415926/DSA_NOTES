# bottom view 
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: Bottom View of Binary Tree
 * ----------------------------------------------------------------------
 * LOGIC:
 * 1. Use TreeMap (Col -> {Row, NodeVal}) to sort by column.
 * 2. DFS: Update map if current node is deeper (row >= stored_row).
 * 3. 'row >= stored' ensures rightmost node is picked for same coordinates.
 * ----------------------------------------------------------------------
 */

public List<Integer> bottomView(TreeNode root) {
    TreeMap<Integer, Integer> map = new TreeMap<>();
    Queue<Pair> q = new LinkedList<>();

    q.offer(new Pair(root, 0));

    while (!q.isEmpty()) {
        Pair p = q.poll();
        TreeNode node = p.node;
        int col = p.col;

        map.put(col, node.val); // overwrite

        if (node.left != null)
            q.offer(new Pair(node.left, col - 1));
        if (node.right != null)
            q.offer(new Pair(node.right, col + 1));
    }

    return new ArrayList<>(map.values());
}


/*
 * COMPLEXITY:
 * Time: O(N log N) (TreeMap ops).
 * Space: O(N) (Map + Recursion stack).
 */
````