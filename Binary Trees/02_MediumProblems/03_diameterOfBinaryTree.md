# diameter of a binary tree

## brute force 
````java
public int height(TreeNode root) {
    // Base case: empty node has 0 height.
    if (root == null) {
        return 0;
    }
    // Height = 1 (current) + max height of children.
    return 1 + Math.max(height(root.left), height(root.right));
}

/**
 * Calculates the diameter of the binary tree.
 */
public int diameterOfBinaryTree(TreeNode root) {
    // Base case: An empty tree has 0 diameter.
    if (root == null) {
        return 0;
    }

    // 1. Diameter *within* the left subtree
    int ld = diameterOfBinaryTree(root.left);
    // 2. Diameter *within* the right subtree
    int rd = diameterOfBinaryTree(root.right);

    // 3. Diameter *passing through* the current root
    // (This counts nodes. LeetCode 543 wants edges, which is this - 1)
    int pathThroughRoot = height(root.left) + height(root.right);

    // Return the max of the three possibilities.
    return Math.max(pathThroughRoot, Math.max(ld, rd));
}

    /*
    📊 Complexity Analysis (Top-Down Solution)

    * Time Complexity: O(n^2) worst case, O(n log n) average
        * We call `height()` (an O(n) operation in the worst case) for every node.
        * Worst Case (Skewed Tree): T(n) = T(n-1) + O(n) -> O(n^2)
        * Average Case (Balanced Tree): T(n) = 2*T(n/2) + O(n) -> O(n log n)
    
    * Space Complexity: O(h) (height of tree)
        * Due to the recursion stack.
        * O(n) in the worst case (skewed tree).
        * O(log n) in the average case (balanced tree).
    */
}
````
## optimal(directly finding in the height function)
````java
public int height(TreeNode root, int[] diam) {
        // Base case: An empty node has 0 height.
        if (root == null) {
            return 0;
        }

        // 1. Recursively find the height of left and right subtrees.
        int lh = height(root.left, diam); // Height of left subtree
        int rh = height(root.right, diam); // Height of right subtree

        // 2. Update the max diameter (Side Effect)
        // The diameter *passing through this node* is (lh + rh).
        // (This counts the edges, as required by the problem).
        // We check if this path is the new global maximum.
        diam[0] = Math.max(diam[0], lh + rh);

        // 3. Return the height of the current node
        return 1 + Math.max(lh, rh);
    }

    /**
     * Calculates the diameter of the binary tree.
     */
    public int diameterOfBinaryTree(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        // Use a 1-element array to pass the diameter by reference.
        // diam[0] will store the maximum diameter found.
        int[] diam = new int[1];
        
        // Start the DFS. The function's return value (height) is ignored here;
        // we only care about the side effect that updates diam[0].
        height(root, diam);
        
        // The final answer is the max diameter stored in the array.
        return diam[0];
    }

    /*
    📊 Complexity Analysis
    * Time Complexity: O(n)
        * We visit every node exactly once, performing constant-time
        * operations (math, comparison) at each node.
    
    * Space Complexity: O(h) (height of tree)
        * This space is used by the recursion call stack.
        * O(n) in the worst case (a skewed tree, like a linked list).
        * O(log n) in the average case (a balanced tree).
    */
}
````