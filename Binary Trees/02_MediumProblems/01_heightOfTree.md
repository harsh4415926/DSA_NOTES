# Height of tree or depth of tree
````java
public int maxDepth(TreeNode root) {
        // Base case: If the tree/subtree is empty, its depth is 0.
        if (root == null) {
            return 0;
        }

        // Recursive step:
        // 1. Calculate the depth of the left subtree.
        int leftDepth = maxDepth(root.left);
        // 2. Calculate the depth of the right subtree.
        int rightDepth = maxDepth(root.right);

        // The depth of the current node is 1 (for itself) plus the
        // depth of its *deepest* child subtree.
        return 1 + Math.max(leftDepth, rightDepth);
    }

    /*
    📊 Complexity Analysis
    * Time Complexity: O(n)
        * We must visit every single node in the tree exactly once to
        * determine the depth. 'n' is the total number of nodes.
    
    * Space Complexity: O(h) or O(log n) on average, O(n) in the worst case
        * This is determined by the height of the recursion stack.
        * - **Best/Average Case (Balanced Tree):** The tree's height 'h' is
        * logarithmic to the number of nodes, so the space is O(log n).
        * - **Worst Case (Skewed Tree):** If the tree is completely unbalanced
        * (like a linked list), the height 'h' is equal to 'n', leading to
        * O(n) space for the recursion stack.
    */
}
````