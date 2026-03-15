# construct binary tree from inorder and preorder
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: Construct Binary Tree from Preorder and Inorder Traversal
 * ----------------------------------------------------------------------
 * LOGIC (Recursion):
 * 1. Preorder (Root, Left, Right): First element is ALWAYS the root.
 * 2. Inorder (Left, Root, Right): Root splits array into Left & Right subtrees.
 * 3. Strategy:
 * - Pick root from Preorder (track using global `preIndex`).
 * - Find root index in Inorder (Map used for O(1)).
 * - Recursively build Left (start to index-1).
 * - Recursively build Right (index+1 to end).
 * ----------------------------------------------------------------------
 */

class Solution {
    int preIndex = 0; // Tracks current root in Preorder array
    HashMap<Integer, Integer> map = new HashMap<>(); // Stores Inorder Value -> Index
    
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        // Cache inorder indices for O(1) lookup
        for(int i = 0; i < inorder.length; i++) {
            map.put(inorder[i], i);
        }
        return build(preorder, 0, inorder.length - 1);
    }
    
    private TreeNode build(int[] preorder, int inStart, int inEnd) {
        // Base Case: Empty subtree
        if(inStart > inEnd) return null;
        
        // Step 1: Get root value from Preorder & increment index
        int rootVal = preorder[preIndex++];
        TreeNode root = new TreeNode(rootVal);
        
        // Step 2: Find split point in Inorder
        int inorderIndex = map.get(rootVal);
        
        // Step 3: Recurse
        // IMPORTANT: Must build LEFT before RIGHT because Preorder is (Root -> Left -> Right)
        root.left = build(preorder, inStart, inorderIndex - 1);
        root.right = build(preorder, inorderIndex + 1, inEnd);
        
        return root;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N) (Map creation + Visiting every node once).
 * Space: O(N) (Map + Recursion Stack).
 */
````

# construct binary tree from inorder and postorder
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: Construct Binary Tree from Inorder and Postorder Traversal
 * ----------------------------------------------------------------------
 * LOGIC (Recursion):
 * 1. Postorder (Left, Right, Root): Last element is ALWAYS the root.
 * 2. Traverse Postorder BACKWARDS (Root -> Right -> Left).
 * 3. Strategy:
 * - Pick root from end of Postorder (decrement global `postIdx`).
 * - Find split index in Inorder (Map for O(1)).
 * - CRITICAL: Build RIGHT subtree FIRST, then LEFT.
 * (Because moving backwards in Postorder hits Right child before Left).
 * ----------------------------------------------------------------------
 */

class Solution {
    HashMap<Integer, Integer> map;
    int postIdx; // Tracks current root in Postorder (moving backwards)

    public TreeNode build(int inStart, int inEnd, int[] inorder, int[] postorder) {
        // Base Case: Empty subtree range
        if (inStart > inEnd) return null;

        // Step 1: Get root value & decrement index (processing Right-to-Left)
        int nodeValue = postorder[postIdx--];
        TreeNode node = new TreeNode(nodeValue);

        // Step 2: Find split point in Inorder
        int inIdx = map.get(nodeValue);

        // Step 3: Recurse
        // MUST build RIGHT first because Postorder end is (.. Left, Right, Root)
        // Previous element in Postorder corresponds to Right child.
        node.right = build(inIdx + 1, inEnd, inorder, postorder);
        node.left = build(inStart, inIdx - 1, inorder, postorder);

        return node;
    }

    public TreeNode buildTree(int[] inorder, int[] postorder) {
        map = new HashMap<>();
        // Cache inorder indices for O(1) lookup
        for (int i = 0; i < inorder.length; i++) {
            map.put(inorder[i], i);
        }
        
        // Start from the very last element of Postorder
        postIdx = postorder.length - 1;
        return build(0, inorder.length - 1, inorder, postorder);
    }
}

/*
 * COMPLEXITY:
 * Time: O(N) (Map creation + Visit every node).
 * Space: O(N) (Map + Recursion Stack).
 */
````