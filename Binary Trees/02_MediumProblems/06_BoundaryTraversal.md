# boundary traversal(gfg)
````java
// Assuming 'Node' class is defined elsewhere, e.g.:
/*
class Node {
    int data;
    Node left, right;

    Node(int item) {
        data = item;
        left = right = null;
    }
}
*/

class Solution {

    /**
     * Helper function to check if a given node is a leaf node.
     * A node is a leaf if it has no left child and no right child.
     *
     * @param root The node to check.
     * @return true if the node is a leaf, false otherwise.
     */
    boolean isLeaf(Node root) {
        return root.left == null && root.right == null;
    }

    /**
     * Recursively traverses and adds the left boundary nodes (top-down).
     * It adds a node to the answer list if it's NOT a leaf.
     * It prioritizes moving to the left child. If no left child exists,
     * it moves to the right child to follow the boundary.
     *
     * @param root The current node in the traversal.
     * @param ans  The list to store the boundary nodes.
     */
    void fillLeftBoundary(Node root, ArrayList<Integer> ans) {
        // Base case: If the node is null, stop recursion.
        if (root == null) return;

        // Add the node's data ONLY if it is not a leaf node.
        if (!isLeaf(root)) {
            ans.add(root.data);
        }

        // Recurse: Prioritize the left subtree.
        if (root.left != null) {
            fillLeftBoundary(root.left, ans);
        }
        // If no left child, move to the right child to continue the boundary.
        else {
            fillLeftBoundary(root.right, ans);
        }
    }

    /**
     * Performs a pre-order traversal to find and add all leaf nodes.
     * It recursively visits all nodes but only adds the data
     * of nodes that are identified as leaves.
     *
     * @param root The current node in the traversal.
     * @param ans  The list to store the boundary nodes.
     */
    void fillLeafNodes(Node root, ArrayList<Integer> ans) {
        // Base case: If the node is null, stop recursion.
        if (root == null) return;

        // If the current node is a leaf, add its data to the list.
        if (isLeaf(root)) {
            ans.add(root.data);
            return; // Stop further recursion from this leaf
        }

        // Recurse on the left and right children to find other leaves.
        fillLeafNodes(root.left, ans);
        fillLeafNodes(root.right, ans);
    }

    /**
     * Iteratively traverses and adds the right boundary nodes (bottom-up).
     * It uses a Stack to reverse the order of the right boundary.
     * It traverses top-down, pushing non-leaf nodes onto the stack.
     * After the traversal, it pops from the stack to add nodes to the
     * answer list in the correct bottom-up order.
     *
     * @param root The current node in the traversal (starts at root.right).
     * @param ans  The list to store the boundary nodes.
     */
    void fillRightBoundary(Node root, ArrayList<Integer> ans) {
        // Base case: If the node is null, stop.
        if (root == null) return;

        Stack<Integer> st = new Stack<>();

        // Traverse down the right boundary.
        while (root != null) {
            // Add the node's data to the stack ONLY if it is not a leaf.
            if (!isLeaf(root)) {
                st.add(root.data);
            }

            // Recurse: Prioritize the right subtree.
            if (root.right != null) {
                root = root.right;
            }
            // If no right child, move to the left child to continue the boundary.
            else {
                root = root.left;
            }
        }

        // Pop all elements from the stack and add them to the answer list.
        // This adds the right boundary in the correct bottom-up order.
        while (!st.isEmpty()) {
            ans.add(st.pop());
        }
    }

    /**
     * Performs an anti-clockwise boundary traversal of the binary tree.
     * The traversal consists of three parts:
     * 1. The root node.
     * 2. The left boundary (excluding leaves).
     * 3. All leaf nodes (from left to right).
     * 4. The right boundary (excluding leaves, in bottom-up order).
     *
     * @param root The root of the binary tree.
     * @return An ArrayList<Integer> containing the nodes in boundary order.
     */
    ArrayList<Integer> boundaryTraversal(Node root) {
        ArrayList<Integer> ans = new ArrayList<>();

        // Base case: If the tree is empty, return an empty list.
        if (root == null) return ans;

        // 1. Add the root node's data.
        // We add it here unless it's a leaf to avoid duplication
        // by fillLeafNodes. But the main function adds it unconditionally
        // and handles the single-node-tree case separately.
        ans.add(root.data);

        // Handle the case where the root is the ONLY node in the tree.
        // If so, it was added above, and we can just return.
        if (root.left == null && root.right == null) return ans;

        // 2. Add the left boundary (top-down, excluding root and leaves).
        // Start from root.left.
        if (root.left != null) {
            fillLeftBoundary(root.left, ans);
        }

        // 3. Add all leaf nodes (left-to-right).
        // Start from the root to find all leaves.
        fillLeafNodes(root, ans);

        // 4. Add the right boundary (bottom-up, excluding root and leaves).
        // Start from root.right.
        if (root.right != null) {
            fillRightBoundary(root.right, ans);
        }

        // Return the final list of boundary nodes.
        return ans;
    }
}
````