# check a binary tree balanced or not 
 balanced if height difference of left and right subtree is less than or equal to 1 and that is true for each node in the tree
 
## brute force (checking height at each node)
````java
public int height(TreeNode root) {
        // Base case: An empty node has a height of 0.
        if (root == null) {
            return 0;
        }
        
        // Recursively find the height of left and right subtrees.
        int leftHeight = height(root.left);
        int rightHeight = height(root.right);
        
        // The height of this node is 1 (for itself) + the max of its children's heights.
        return 1 + Math.max(leftHeight, rightHeight);
    }

    /**
     * Checks if the tree rooted at 'root' is height-balanced.
     */
    public boolean isBalanced(TreeNode root) {
        // Base case: An empty tree (null node) is considered balanced.
        if (root == null) {
            return true;
        }

        // 1. Check balance at the CURRENT node
        // Get the height of the entire left subtree.
        int lh = height(root.left);
        // Get the height of the entire right subtree.
        int rh = height(root.right);
        
        // If the heights differ by more than 1, this tree is unbalanced.
        if (Math.abs(lh - rh) > 1) {
            return false;
        }

        // 2. Check if subtrees are ALSO balanced
        // The current node is balanced, but we must also ensure *all*
        // sub-nodes are balanced.
        return isBalanced(root.left) && isBalanced(root.right);
    }

    /*
    📊 Complexity Analysis (For *this* specific top-down solution)

    * Time Complexity: O(n log n) average, O(n^2) worst case
        * **Worst Case (Skewed Tree):** The recurrence relation is
        * T(n) = T(n-1) + O(n). The O(n) part comes from the `height()` call,
        * which traverses the entire subtree. This solves to O(n^2).
        * **Average Case (Balanced Tree):** The recurrence is
        * T(n) = 2*T(n/2) + O(n). The O(n) is from `height()`. This solves to
        * O(n log n) by the Master Theorem.
    
    * Space Complexity: O(h) or O(n) worst case
        * The space is determined by the maximum depth of the recursion stack.
        * In a balanced tree, height 'h' is O(log n).
        * In a skewed tree (worst case), height 'h' is O(n).
    */

````

## optimed (using the height function)
````java
// Helper function:
//    Returns the height of the tree if it's balanced.
//     Returns -1 if it's unbalanced.
        
        public int helper(TreeNode root) {
        // Base case: An empty node has a height of 0 and is balanced.
        if (root == null) {
        return 0;
        }

    // Recursively find the "height" of the left subtree.
    // This will be -1 if the left subtree is unbalanced.
    int leftHeight = helper(root.left);

    // Recursively find the "height" of the right subtree.
    // This will be -1 if the right subtree is unbalanced.
    int rightHeight = helper(root.right);

    // Check for unbalanced conditions:
    // 1. If left subtree was unbalanced
    // 2. If right subtree was unbalanced
    // 3. If the *current* node is unbalanced (height difference > 1)
    if (leftHeight == -1 || rightHeight == -1 || Math.abs(leftHeight - rightHeight) > 1) {
    return -1; // Propagate the "unbalanced" signal up
    }

    // If all checks passed, the current subtree is balanced.
    // Return its actual height.
    return 1 + Math.max(leftHeight, rightHeight);
    }

  /**
    * Checks if the tree rooted at 'root' is height-balanced.
      */
      public boolean isBalanced(TreeNode root) {
      // We just need to call the helper.
      // If it returns -1, the tree is unbalanced.
      if (helper(root) == -1) {
      return false;
      }
      // Otherwise (it returned 0 or a positive height), it's balanced.
      return true;
      }

  /*
  📊 Complexity Analysis
    * Time Complexity: O(n)
        * This algorithm visits every node exactly once, from the bottom up.
        * Unlike the top-down approach, it does not re-calculate heights for
        * the same nodes.

    * Space Complexity: O(h) or O(log n) on average, O(n) in the worst case
        * The space is determined by the maximum depth of the recursion stack.
        * - **Best/Average Case (Balanced Tree):** The tree's height 'h' is
        * logarithmic to the number of nodes, so the space is O(log n).
        * - **Worst Case (Skewed Tree):** If the tree is completely unbalanced
        * (like a linked list), the height 'h' is equal to 'n', leading to
        * O(n) space for the recursion stack.
          */

````