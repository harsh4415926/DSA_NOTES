# lowest common ancestor
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: Lowest Common Ancestor (LCA) of Binary Tree
 * ----------------------------------------------------------------------
 * LOGIC (DFS / Post-Order):
 * 1. Base Case: If root is null or equals p/q, return root.
 * 2. Recurse left and right.
 * 3. Result:
 * - If both left & right return valid nodes -> Current node is LCA.
 * - If only one returns valid -> Propagate that node up.
 * ----------------------------------------------------------------------
 */

class Solution {
    public TreeNode helper(TreeNode root, TreeNode p, TreeNode q) {
        // Base case: Reached end or found one of the targets
        if (root == null) return null;
        if (root == p || root == q) return root;
        
        // Search in subtrees
        TreeNode left = helper(root.left, p, q);
        TreeNode right = helper(root.right, p, q);
        
        // Key Logic:
        // If both sides returned a node, p and q are in different branches.
        // Thus, current 'root' is the split point (LCA).
        if (left != null && right != null) return root;
        
        // Otherwise, return the non-null child (bubbling up the found node)
        // If both are null, returns null.
        if (left != null) return left;
        else return right;
    }

    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        return helper(root, p, q);
    }
}

/*
 * COMPLEXITY:
 * Time: O(N) (Visit every node in worst case).
 * Space: O(H) (Recursion stack height).
 */
````