# serializing and deserializing binary tree
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: Serialize and Deserialize Binary Tree (LeetCode 297)
 * ----------------------------------------------------------------------
 * LOGIC (Preorder Traversal):
 * 1. Serialization:
 * - Use Preorder (Root -> Left -> Right). (can use any traversal)
 * - Append "n" (or any marker) for null nodes to preserve structure.
 * - Use a delimiter (e.g., ",") between values.
 *
 * 2. Deserialization:
 * - Split string by delimiter to get node values.
 * - Use a global index (`idx`) to traverse the values.
 * - Recursive Build:
 * - If current value is "n", return null & increment index.
 * - Else, create node, increment index.
 * - Recursively build Left child, then Right child.
 * ----------------------------------------------------------------------
 */

public class Codec {
    
    // Helper: Recursive Preorder Traversal for Serialization
    public void preorder(TreeNode root, StringBuilder sb) {
        if (root == null) {
            sb.append("n,"); // 'n' represents null
            return;
        }
        sb.append(root.val + ",");
        preorder(root.left, sb);
        preorder(root.right, sb);
    }

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if (root == null) return "n"; // Handle edge case for empty tree
        
        StringBuilder sb = new StringBuilder();
        preorder(root, sb);
        
        // Remove trailing comma if exists (cosmetic, but good practice)
        if (sb.length() > 0 && sb.charAt(sb.length() - 1) == ',') {
            sb.deleteCharAt(sb.length() - 1);
        }
        return sb.toString();
    }

    // Decodes your encoded data to tree.
    int idx = 0; // Global index tracker for recursion
    
    public TreeNode deserialize(String data) {
        if (data.equals("n")) return null; // Handle empty tree case
        
        String[] preorder = data.split(",");
        idx = 0;
        return buildPreorder(preorder);
    }
     
    // Helper: Recursive Build from Preorder Array
    public TreeNode buildPreorder(String[] preorder) {
        // Boundary check (though logic usually prevents OOB)
        if (idx >= preorder.length) return null;
        
        // Base Case: Null marker found
        if (preorder[idx].equals("n")) {
            idx++; 
            return null;
        }
        
        // Create Node
        TreeNode node = new TreeNode(Integer.parseInt(preorder[idx++])); // Post-increment idx
        
        // Recursively build Left, then Right
        node.left = buildPreorder(preorder);
        node.right = buildPreorder(preorder);
        
        return node;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N) (Visit every node once).
 * Space: O(N) (Recursion stack + String storage).
 */
````